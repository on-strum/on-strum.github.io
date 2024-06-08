# Usage

To have an access for `OnStrum::Logs` you must configure it first as in the example below:

## Configuration

### Setting custom configuration

```ruby
# config/initializers/on_strum_logs.rb

require 'on_strum/logs'

OnStrum::Logs.configure do |config|
  # Optional parameter. Ability to define static fields in the logger document root.
  # It is equal to empty hash by default.
  config.root_fields = {
    service_name: 'My Great Application',
    service_version: '1.42.0'
  }

  # Optional parameter. The colorized structured log in STDOUT. It could be useful
  # for debug mode.
  config.detailed_formatter = true

  # Optional parameter. You can use your custom formatter insted of built-in.
  # Please note, using this option will override using detailed_formatter option.
  config.custom_formatter = YourCustomFormatter

  # Optional parameter. You can override log level builtin attribute key.
  # It is equal to :level by default.
  config.field_name_level = :log_level

  # Optional parameter. You can override log time builtin attribute key.
  # It is equal to :time by default.
  config.field_name_time = :log_time

  # Optional parameter. You can override log message builtin attribute key.
  # It is equal to :message by default.
  config.field_name_message = :log_message

  # Optional parameter. You can override log context builtin attribute key.
  # It is equal to :context by default.
  config.field_name_context = :log_context

  # Optional parameter. You can override log exception message builtin attribute key.
  # It is equal to :message by default.
  config.field_name_exception_message = :log_exception_message

  # Optional parameter. You can override log exception stack trace builtin attribute key.
  # It is equal to :stack_trace by default.
  config.field_name_exception_stack_trace = :log_exception_stack_trace
end
```

### Custom formatter

Example of defining custom formatter:

```ruby
YourCustomFormatter = Class.new do
  def self.call(**log_data)
    log_data
  end
end
```

### Setting default configuration

You able to use default configuration. All configuration attributes will be with default values.

```ruby
# config/initializers/on_strum_logs.rb

require 'on_strum/logs'

OnStrum::Logs.configure {}
```

### Reading configuration

After successful configuration, you can read current `OnStrum::Logs` configuration instance anywhere in your application.

```ruby
OnStrum::Logs.configuration
=> #<OnStrum::Logs::Configuration:0x000000011d59bb40
 @field_name_context=:context,
 @field_name_exception_message=:message,
 @field_name_exception_stack_trace=:stack_trace,
 @field_name_level=:level,
 @field_name_message=:message,
 @field_name_time=:time,
 @root_fields={}>
```

### Reseting configuration

Also you can reset `OnStrum::Logs` configuration.

```ruby
OnStrum::Logs.reset_configuration!
=> nil
OnStrum::Logs.configuration
=> nil
```

## Use cases

In `OnStrum::Logs` supports 3 standard log levels: `INFO`, `ERROR` and `DEBUG`.

```ruby
OnStrum::Logs.info(object)
OnStrum::Logs.error(object)
OnStrum::Logs.debug(object)
```

As methods argument you can use instance of `Hash`, `Exception` or any other class (it will be stringified).

### Hash

Please note, when you uses `Hash`, `:message` is required key. Otherwise you will get an exception.

```ruby
OnStrum::Logs.info(message: 'My Message', some_attribute: 'some attribute')
```

```json
{
  "level": "INFO",
  "time":" 2023-10-15T13:15:21.129+02:00",
  "message": "My Message",
  "context": {
    "some_attribute": "some attribute"
  },
  "service_name": "My Great Application",
  "service_version": "1.42.0"
}
```

### Exception

```ruby
OnStrum::Logs.error(StandardError.new('error message'))
```

```json
{
  "level": "ERROR",
  "time": "2023-10-15T13:32:15.851+02:00",
  "message": "Exception: StandardError",
  "context": {
    "message": "error message",
    "stack_trace": null
  },
  "service_name": "My Great Application",
  "service_version": "1.42.0"
}
```

### Other

```ruby
OnStrum::Logs.debug(42)
```

```json
{
  "level": "DEBUG",
  "time": "2023-10-15T13:33:51.889+02:00",
  "message": "42",
  "context": null,
  "service_name": "My Great Application",
  "service_version": "1.42.0"
}
```

### Debug mode

For view detailed colorized logs you can use configuration option `detailed_formatter = true`:

```ruby
require 'on_strum/logs'

OnStrum::Logs.configure do |config|
  config.root_fields = {
    service_name: 'My Great Application',
    service_version: '1.42.0'
  }
  config.detailed_formatter = true
end

OnStrum::Logs.info(
  message: 'My Message',
  attribute_1: 'attribute 1',
  attribute_2: {
    a: 42,
    b: Class.new
  }
)
```

```bash
{
            :level => "INFO",
            :time  => 2023-10-15 14:02:11.441533 +0200,
          :message => "My Message",
          :context => {
      :attribute_1 => "attribute 1",
      :attribute_2 => {
          :a => 42,
          :b => #<Class:0x0000000103763528> < Object
      }
  },
      :service_name => "My Great Application",
   :service_version => "1.42.0"
}
```
