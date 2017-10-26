## Frameworks
- configure (browser etc.)
- setup (date etc.)
- action
- test (assertions)
- teardown
- cleanup

---

## Do clients need to see it?
- Should be able to show your tests to someone, your manager? and they should be able to read it, without having to know how to code
- you can do this by using something like cucumber, spec, etc
- to do this well you need to abstract things out to page objects, models, etc and have data sources external to your tests

---

### If you do, make sure it's readable
For example, instead of
```ruby
it 'Allows users to sign up' do
  fname = Faker::FirstName.new
  lname = Faker::LastName.new
  uname = Faker::Username.new
  passw = Faker::Password.new
  user = [fname, lname, uname, passw]
  @browser = BrowserDriver.new(:netscape)
  @browser.goto("http://alpha.my_test_app.com/")
  @browser.button(text: 'Sign Up').wait_until_present
  @browser.button(text: 'Sign Up').click
  @browser.form(id: 'signUpForm').wait_for(:&present?)
  form_ids = ['First Name', 'Last Name', 'Username', 'Password']
  form_ids.each_with_index { |id| @browser.text_field(id: id).set(user[i]) }
  # @browser.text_field(id: 'First Name').set(fname)
  # @browser.text_field(id: 'Last Name').set(lname)
  # @browser.text_field(id: 'Username').set(uname)
  # @browser.text_field(id: 'Password').set(passw)
  @browser.button(text: 'Submit').click
  @browser.wait_for_ajax
  @browser.navigate_to(LoginPage)
  @browser.form(id: 'login').wait_for(:&present?)
  @browser.form.text_field.first.set(fname)
  @browser.form.text_fields[1].set(lname)
  @browser.form.text_field(css: '#login > div > span.user > input:nth-child(2)').set(passw)
  # blah
  test['sign_up'] = @browser.blahblahblah.text == fname ? true : false
  # blah
  # blarg!
end
```

---

use
```ruby
it 'Allows users to sign up' do
  user = UserManager.create
  visit(SignUpPage)
  SignUp(user)
  verify(UserSignedUp?(user) == true)
  UserManager.remove(user)
end

Class UserManager
  def self.create
    Faker::User.new
  end

  def remove(user)
    RestClient::Delete(USER_MANAGEMENT_API_URL, user['UserName'])
  end
end
```

---

## Reporting

---

## Acting on errors

---

## Maintenence

---
