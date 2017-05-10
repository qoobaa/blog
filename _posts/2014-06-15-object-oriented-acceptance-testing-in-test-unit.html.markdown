---
title: Object Oriented Acceptance Testing in Test::Unit
description: The article describes how to use object oriented acceptance tests approach in Test::Unit (à la BBQ)
author: Kuba Kuźma
hacker_news_id: 7895100
---

In a few previous projects that I was doing recently, I decided to
write acceptance tests in a little bit more object oriented
manner. The idea comes from a
[BBQ gem](https://github.com/drugpl/bbq), implemented by guys from
[DRUG](http://drug.org.pl/). The gem seems to be a really good
solution for most of the cases. It supports `RSpec` and `Test::Unit`,
provides helpers, generators and some more useful stuff. However, it
turns out that the whole idea is pretty easy to implement on your own,
if you cannot use the gem for some reason (i.e. in an unsupported
testing framework).

The first thing that you need to do, is to implement your testing
personas. Instead of controlling a global browser instance, we would
like it to be bound to a particular user object. It would be nice if
such an object behaves pretty much like our `User` model, but have
some additional abilities at the same time. This behaviour can be
easily achieved using standard Ruby `SimpleDelegator` class.

~~~ ruby
class Test::User < SimpleDelegator
  attr_reader :session, :model

  def initialize(user)
    super
    @model = user
    @session = Capybara::Session.new(:poltergeist, Rails.application)
  end
end
~~~

From now on we can use user's attributes and associations just like we
did before (`user.email`, `user.orders`), but we also have user
browser `session` available, and bound to the object. It would be nice
to have all Capybara's DSL methods available on the `Test::User`
object as well. Unfortunately Capybara does not provide a single
module that we could include in our class, so we need to iterate
through `DSL_METHODS` array, just like
[Capybara does](https://github.com/jnicklas/capybara/blob/26f6c783a71b16e3ff82443bbca4e45bab3fa758/lib/capybara/dsl.rb#L49).

~~~ruby
class Test::User
  Capybara::Session::DSL_METHODS.each do |method|
    define_method method do |*args, &block|
      session.send method, *args, &block
    end
  end
end
~~~

Now we are able to drive the browser directly through the user object,
i.e. `user.visit("/")`, `user.click_on("Sign in")`. It would also be
handy to have URL helpers and FactoryGirl methods available in the
object.

~~~ruby
class Test::User
  include Rails.application.routes.url_helpers
  include FactoryGirl::Syntax::Methods
end
~~~

We can also define our own helper methods.

~~~ruby
class Test::User
  def see?(*args)
    has_content?(*args)
  end

  def emails
    ActionMailer::Base.deliveries.select { |mail| mail.to.include?(email) }
  end
end
~~~

And inherit a generic `User` class creating more specific classes with
higher level, role specific methods.

~~~ruby
class Test::Client < Test::User
  def initialize(user = FactoryGirl.create(:client))
    super
  end

  def sign_in
    visit(root_path)
    click_on("Sign In")
    fill_in("Email", with: email)
    fill_in("Password", with: password)
    click_on("Sign In")
  end
end
~~~

This approach allows you to write more readable, object oriented
tests. It is much easier to test interactions between users, since
you can create multiple browser sessions within a single test.

~~~ruby
class GatekeeperAcceptanceTest < ActionDispatch::IntegrationTest
  test "receives an email with a reservation request" do
    agent = Test::Gatekeeper.new
    gatekeeper.sign_in
    gatekeeper.go_on_duty
    assert_difference("gatekeeper.emails.size") do
      client = Test::Client.new
      client.sign_in
      client.request_booking(gatekeeper.restaurant.name)
      assert client.see?("Thank you")
    end
  end
end
~~~

One more thing needs to be mentioned before you start using the above
approach in your tests. The example uses
[Poltergeist](https://github.com/teampoltergeist/poltergeist) driver,
and it creates a new browser instance every time you call
`Test::User.new` in your tests. Each browser created in such a way is
a separate process, that stays in the memory until the whole test
suite is finished. It means that you will quickly run out of memory,
causing your tests crash or slowing them down significantly. I suggest
creating a simple `SeessionPool` mechanism to reduce memory footprint
of your test suite.

~~~ruby
module Test::SessionPool
  mattr_accessor :idle, :taken

  self.idle = []
  self.taken = []

  def self.release
    idle.concat(taken)
    taken.clear
  end

  def self.take
    (idle.pop || Capybara::Session.new(:poltergeist, Rails.application)).tap do |session|
      session.reset!
      taken.push(session)
    end
  end
end
~~~

Every time when you instantiate a new user, it is better to take a
session from the pool instead of creating a new one.

~~~ruby
class Test::User < SimpleDelegator
  def initialize(user)
    super
    @model = user
    @session = Test::SessionPool.take
  end
end
~~~

Do not forget to release taken sessions after your test finishes.

~~~ruby
class ActionDispatch::IntegrationTest
  teardown do
    Test::SessionPool.release
  end
end
~~~
