# WebApiExtension
[![Latest Stable Version](https://poser.pugx.org/linio/behat-web-api-extension/v/stable.svg)](https://packagist.org/packages/linio/behat-web-api-extension) 
[![License](https://poser.pugx.org/linio/behat-web-api-extension/license.svg)](https://packagist.org/packages/linio/behat-web-api-extension) 
![Build Status](https://github.com/LinioIT/behat-web-api-extension/actions/workflows/build.yml/badge.svg)

Provides testing for REST APIs with Behat 3. This is a maintained fork
of `behat/web-api-extension` with additional features and long term support.

## Usage
Just add to your composer development dependencies:

    $ composer require --dev linio/behat-web-api-extension

And activate your extension:

    # behat.yml
    default:
      # ...
      extensions:
        Behat\WebApiExtension: ~

## Private to protected
One of the tricky things in the original `behat/web-api-extension` library is
the extensive use of `private` properties and methods, preventing you from
easily extending it. This fork fixes it by moving everything to `protected`.

## Placeholder support
One of the new features from this fork is the ability to use placeholders with
regular expressions to help you test input or output that varies. For example:

```
  Scenario: Sending values with placeholders
    Given a file named "features/send_values.feature" with:
      """
      Feature: Exercise WebApiContext data sending
        In order to validate the send request step
        As a context developer
        I need to be able to send a request with values in a scenario

        Scenario:
          When I send a POST request to "echo" with values:
          | name | name |
          | pass | pass |
          Then the response should contain "POST"
          And the response should contain json:
          '''
          {
          "name" : "name",
          "pass": "%[a-z]+%"
          }
          '''
      """
    When I run "behat features/send_values.feature"
    Then it should pass with:
      """
      ...

      1 scenario (1 passed)
      """
```

It is common for APIs to return responses with dynamic content. UUIDs,
timestamps, generated passwords, etc. All of those, unfortunately, make
writing scenarios a bit challenging. With placeholders, you can easily
test by using regular expressions to ensure they are returned in a valid
format, but can still be variable.

Other examples of placeholders:

```
{
  "timestamp": "%^[0-9]{4}-[0-9]{2}-[0-9]{2}T[0-9]{2}:[0-9]{2}:[0-9]{2}.[0-9]{2,}[\\-\\+][0-9]{2}:[0-9]{2}$%",
  "uuid": "%^[0-9a-fA-F]{8}-([0-9a-fA-F]{4}-){3}[0-9a-fA-F]{12}$%"
}
```

## Tests
    $ composer install
    $ php -S 0.0.0.0:8080 -t testapp &
    $ vendor/bin/behat -f progress

## Copyright
Copyright (c) 2014 Konstantin Kudryashov (ever.zet). See LICENSE for details.

## Contributors
* Christophe Coevoet [stof](http://github.com/stof) [lead developer]
* Other [awesome developers](https://github.com/Behat/WebApiExtension/graphs/contributors)
