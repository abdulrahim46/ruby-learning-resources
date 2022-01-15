
This may become its own repo. For now including in ruby-learning-resources


The purpose is to collect rails tid-bits that will serve as a useful reminder for anyone forgetful. 


things to include: fixtures, factories, testing (defaults, but also with RSpec). 


# Intro

- The average rails app has 1.03 lines of test code for every line of code in the app! [Source](https://semaphoreci.com/blog/2018/04/11/state-of-testing-in-rails.html)



# Incredibly important resources:

- Railsdiff (explained [here](https://www.youtube.com/watch?v=ljgmjoIv4Y8&t=5m46s), link [here](https://railsdiff.org/)) - use it when upgrading from one rails version to another.
- How to look up rails documentation: [api.rubyonrails.org](https://api.rubyonrails.org/classes/ActiveRecord/ConnectionAdapters/Table.html#method-i-timestamps) - **use the search bar on top left**
- [Rails guides](https://guides.rubyonrails.org/) (these aren't 'read once and forget' - experienced devs use them daily)


# How to setup your working environment (IDE, terminal etc):

- Great video on vscode extensions [here](https://www.youtube.com/watch?v=VXTmd-4jx8g)



# Notes from [Rails 7 demo](https://www.youtube.com/watch?v=mpWFrUwAN88)

- `.deliver_later` knows to run inline in development, and for it to work properly (i.e. asynchonously) in production, it requires some setup of (??) 
- Rich text fields
- 








# Scopes

In your controller you might want to only show published posts: 

```ruby

def index

  @posts = Post.all.where(status: "Published")

end
```

Another (more elegant) way to do this is jumping into your Post model and creating a scope (the `scope` method appears to accept 2 arguments, a name for the scope, and a lambda that contains the code for the scope), such that:


```ruby

scope :published, -> { where(status: "Published") }

```

Then in your controller you can have this:


```ruby

@posts = Post.published

```

And it will do the exact same thing. 

So TL;DR scopes are a great way to keep controller code DRY, and move some controller code into the model. 



Note: **Avoid** using `default_scope`s - they tend to [make a massive mess](https://stackoverflow.com/questions/25087336/why-is-using-the-rails-default-scope-often-recommend-against)!. 















# Testing 

- Awesome [rails guide on testing](https://guides.rubyonrails.org/testing.html)
  - It covers **unit**, **functional**, **integration**, and **system** tests
- Nice reinteractive [article](https://reinteractive.com/posts/342-what-constitutes-good-testing-in-rails) on what constitutes good testing


The purpose of testing is to:

> ensure your code adheres to the desired functionality even after some major code refactoring.

> Rails tests can also simulate browser requests and thus you can test your application's response without having to test it through your browser.


### Types of tests

The rails guide [introduces 4 types of tests](https://guides.rubyonrails.org/testing.html):

> How to write unit, functional, integration, and system tests for your application.

1. Unit tests 
  - e.g. testing minute details in a methods inputs/outputs
  - A unit test is about testing a single function/method in isolation with all of its possible outputs. ([source](https://developers.forem.com/tests/unit-functional-tests))
2. Functional tests 
  - 'testing that controllers and models are using the mailer in the right way'
  - A functional test is about testing a single functionality, which can span multiple methods and a controller. Other common terms in Rails are "model tests," "controller tests," and others. ([source](https://developers.forem.com/tests/unit-functional-tests))
3. Integration tests
  - Integration tests are used to test how various parts of our application interact. They are generally used to test important workflows within our application. ([source](https://guides.rubyonrails.org/testing.html#integration-testing))
  - An integration test is a test that measures the interaction of multiple systems or parts of your application. ([source](https://developers.forem.com/tests/integration-tests))
4. System tests ([AKA **acceptance tests**, **feature tests**](https://developers.forem.com/tests/acceptance-tests#:~:text=Acceptance%20tests%20are%20tests%20from,actions%20inside%20of%20our%20tests.&text=Acceptance%20tests%20can%20be%20found%20in%20the%20directory%20spec%2Fsystem%20.))
  - Might use capibara (a ruby tool for managing selenium) to visit a certain route, and see if it can see what it expects on the page (see great example [here](https://www.youtube.com/watch?v=ZPcRiPrpQTc))
  - System tests allow you to test user interactions with your application, running tests in either a real or a headless browser. System tests use Capybara under the hood. ([source](https://guides.rubyonrails.org/testing.html#system-testing))

Other notes: 

- Acceptance tests are also known as Feature tests or System tests. ([source](https://developers.forem.com/tests/acceptance-tests#:~:text=Acceptance%20tests%20are%20tests%20from,actions%20inside%20of%20our%20tests.&text=Acceptance%20tests%20can%20be%20found%20in%20the%20directory%20spec%2Fsystem%20.))

On mailer tests: 

> There are two aspects of testing your mailer, the **unit tests** and the **functional tests**. In the unit tests, you run the mailer in isolation with tightly controlled inputs and compare the output to a known value (a fixture). In the functional tests you don't so much test the minute details produced by the mailer; instead, we test that our controllers and models are using the mailer in the right way. You test to prove that the right email was sent at the right time.

> Functional tests do not verify whether the specified request type is accepted by the action, we're more concerned with the result. Request tests exist for this use case to make your tests more purposeful. ([source](https://guides.rubyonrails.org/testing.html#available-request-types-for-functional-tests))


### What is acceptance testing? 

From top of google [results](https://developers.forem.com/tests/acceptance-tests#:~:text=Acceptance%20tests%20are%20tests%20from,actions%20inside%20of%20our%20tests.&text=Acceptance%20tests%20can%20be%20found%20in%20the%20directory%20spec%2Fsystem%20.): 

> **Acceptance tests are tests from the perspective of the end-user**. In the Rails world, we sometimes refer to these as **Feature tests** or **System tests**. A tool called Capybara is included to help us simulate a human's actions inside of our tests.

From the [RSpec cheat sheet on acceptance tests](https://thoughtbot.com/upcase/test-driven-rails-resources/rspec_acceptance.pdf). Very elucidating example [here](https://thoughtbot.com/upcase/test-driven-rails-resources/rspec_acceptance.pdf). 


From Jason K: 

> You might have an acceptance test to make sure that when that route is visited, the db is 'fixtured up' and something displayed in the view

From Jason S ([here](https://www.codewithjason.com/set-rails-application-testing/)):

> Capybara is a tool for writing acceptance tests, i.e. tests that interact with the browser and simulate clicks and keystrokes. The underlying tool that allows us to simulate user input in the browser is called Selenium. Capybara allows us to control Selenium using Ruby.

- Brilliant hello world example video [here](https://www.youtube.com/watch?v=ZPcRiPrpQTc)



### Quick tips on testing

From reinteractive's [article](https://reinteractive.com/posts/342-what-constitutes-good-testing-in-rails):

- What's the best framework? Ruby has minitest and Rspec; both are great, just be sure to use a framework - don't go writing your own DSL or doing testing in raw ruby code. 

Testing in essence:: 

> you want to check if something returns the expected results for a known input


**On TDD**

> I am personally not a hardcore fan of any of those camps. Sometimes I write the test before I write the actual code. However, there are also times when I write the code first and then the test. The important part is that I have a test for my new feature. Whether you write the test before or after doesn't really matter, as long as it is there by the time you push your code to review and merge it to the main codebase.


To what extent should you test? 

Author: 

> I'll test the 'happy path' (both arguments provided), possible error paths (e.g. both arguments `nil`), and with one argument as `nil`. 

It would look like this:

```ruby

def full_name
  "#{first_name} #{last_name}".strip 
end


describe "#full_name" do
    let(:user) { User.new(first_name: first_name, last_name: last_name) }

    context "with first name and last name" do
      let!(:first_name) { 'sylvester' }
      let!(:last_name) { 'stallone' }

      it "shows the full name" do
        expect(user.full_name).to eq('sylvester stallone')
      end
    end

    context "when both first and last names are nil" do
      let!(:first_name) { nil }
      let!(:last_name) { nil }

      it "shows the full name" do
        expect(user.full_name).to eq('')
      end
    end

    context "when only one name is present" do
      let!(:first_name) { 'sylvester' }
      let!(:last_name) { nil }

      it "shows the full name" do
        expect(user.full_name).to eq('sylvester')
      end
    end
  end
```

> If it's not critical code, test the 'happy path', the worst case scenario, and one other test. 


Don't test frameworks/libraries (trust their maintainers and the community to do that).

**Validation tests**

Use [shoulda gem](https://github.com/thoughtbot/shoulda) (some more examples [here](https://github.com/thoughtbot/shoulda#overview)):


```ruby
# spec/models/user_spec.rb
 describe '#validations' do
   it { should validate_presence_of(:first_name) }
 end
```



Always write readable tests: 

> Tests usually serve as documentation for your code. Try to write them in a way that someone can understand the intention of the code simply by reading your tests.

> with an existing project, the first thing most of us will do is to run the test suite to see if it passes.


You can run `rspec spec --format documentation` to print the documentation when running tests. It's good practice to use the documentation command above to check whether your spec is readable.


















## RSpec

What does `let` do? `let` simply creates a variable [but it lazy evaluates it](https://pololu.github.io/rpicsim/file.IntroductionToRSpec.html#label-Let+variables), in other words, it only evaluates it when the variable is actually used. I'm pretty sure it memoizes (which is approximately equal to 'caches') it too, so it's kinda 'made once, used many times' rather than being recreated over and over. 





## Factories and Fixtures

From [here](https://stackoverflow.com/questions/7786207/whats-the-difference-between-a-fixture-and-a-factory-in-my-unit-tests):

<blockquote>
  A Fixture is "the fixed state used as a baseline for running tests in software testing"
  A Factory is "an object for creating other objects"
</blockquote>


From [rails guide on testing](https://guides.rubyonrails.org/testing.html#what-are-fixtures-questionmark):

> What are fixtures? **Fixtures is a fancy word for sample data**. Fixtures allow you to populate your testing database with predefined data before your tests run. Fixtures are database independent and written in YAML. There is one file per model.


> YAML-formatted fixtures are a human-friendly way to describe your sample data.


Example:

```yaml
# lo & behold! I am a YAML comment!
david:
  name: David Heinemeier Hansson
  birthday: 1979-10-15
  profession: Systems development

steve:
  name: Steve Ross Kellock
  birthday: 1974-09-27
  profession: guy with keyboard
```


### Faker


Allows us to change this:

```ruby
FactoryBot.define do
  factory :customer do
    first_name { "MyString" }
    last_name { "MyString" }
    email { "MyString" }
  end
end
```

to this: 

```ruby
FactoryBot.define do
  factory :customer do
    first_name { Faker::Name.first_name }
    last_name { Faker::Name.last_name }
    email { Faker::Internet.email }
  end
end
```

That solves the problem of any unique constraints we have in the db (e.g. 'email' field might have the requirement that it's unique). Example from [here](https://www.codewithjason.com/set-rails-application-testing/). 










# Summary of notes from Agile Web Development with Rails 5


How to think of classes and objects in an object oriented language like ruby: 

> When you write object-oriented code, you're normally looking to model concepts from the real world. Typically, during this modelling process you discover categories of thinks that need to be represented. In an online  store, the concept of a line item could be such a category. In Ruby, you'd define a *class* to represent each of these categories. You then use this class as a kind of factory that generates *objects* - instances of that class. An object is a combination of state (for example, the quantity and the product ID) and methods that use the state (perhaps a method to calculate the line the line item's total cost. (p47)

- You create objects by calling a *constructor*, a special method associated with a class. The standard constructor is called `new()`. 
- You invoke methods by sending a message to an object. The message contains the method's name, along with any parameters the method may need. 
- Rails uses *symbols* to identify things. In particular, it uses them as keys when naming method parameters and looking things up in hashes. 
  - A symbol looks like a variable name, but it's prefixed with a colon. You can think of symbols as string literals magically made into constants. Alternatively, you can consider the colon to mean *thing named*, so `:id` is the thing named `id`. 





















