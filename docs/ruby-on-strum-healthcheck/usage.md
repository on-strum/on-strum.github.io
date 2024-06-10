# Usage

To have an access for `OnStrum::Healthcheck` you must configure it first as in the example below:

## Configuration

### Setting custom configuration

```ruby
# config/initializers/on_strum_healthcheck.rb

require 'on_strum/healthcheck'

OnStrum::Healthcheck.configure do |config|
  # Optional parameter. The list of services that can be triggered
  # during running probes. Each value of this hash should be callable
  # and return boolean. It is equal to empty hash by default.
  config.services = {
    postgres: -> { true },
    redis: -> { true },
    rabbit: -> { false }
  }

  # Optional parameter. The list of services that will be checked
  # during running startup probe. As array items must be used an
  # existing keys, defined in config.services.
  # It is equal to empty array by default.
  config.services_startup = %i[postgres]

  # Optional parameter. The list of services that will be checked
  # during running liveness probe. As array items must be used an
  # existing keys, defined in config.services.
  # It is equal to empty array by default.
  config.services_liveness = %i[redis]

  # Optional parameter. The list of services that will be checked
  # during running liveness probe. As array items must be used an
  # existing keys, defined in config.services.
  # It is equal to empty array by default.
  config.services_readiness = %i[postgres redis rabbit]

  # Optional parameter. The name of middleware's root
  # endpoints namespace. Use '/' if you want to use root
  # namespace. It is equal to /healthcheck by default.
  config.endpoints_namespace = '/application-healthcheck'

  # Optional parameter. The startup endpoint path.
  # It is equal to /startup by default.
  config.endpoint_startup = '/startup-probe'

  # Optional parameter. The liveness endpoint path.
  # It is equal to /liveness by default.
  config.endpoint_liveness = '/liveness-probe'

  # Optional parameter. The readiness endpoint path.
  # It is equal to /readiness by default.
  config.endpoint_readiness = '/readiness-probe'

  # Optional parameter. The HTTP successful status
  # for startup probe. It is equal to 200 by default.
  config.endpoint_startup_status_success = 201

  # Optional parameter. The HTTP successful status
  # for liveness probe. It is equal to 200 by default.
  config.endpoint_liveness_status_success = 202
  
  # Optional parameter. The HTTP successful status
  # for readiness probe. It is equal to 200 by default.
  config.endpoint_readiness_status_success = 203

  # Optional parameter. The HTTP failure status
  # for startup probe. It is equal to 500 by default.
  config.endpoint_startup_status_failure = 501

  # Optional parameter. The HTTP failure status
  # for liveness probe. It is equal to 500 by default.
  config.endpoint_liveness_status_failure = 502

  # Optional parameter. The HTTP failure status
  # for readiness probe. It is equal to 500 by default.
  config.endpoint_readiness_status_failure = 503
end
```

### Setting default configuration

You able to use default configuration. All configuration attributes will be with default values.

```ruby
# config/initializers/on_strum_healthcheck.rb

require 'on_strum/healthcheck'

OnStrum::Healthcheck.configure
```

### Reading configuration

After successful configuration, you can read current `OnStrum::Healthcheck` configuration instance anywhere in your application.

```ruby
OnStrum::Healthcheck.configuration
=> #<OnStrum::Healthcheck::Configuration:0x00000001033481a0
 @endpoint_liveness="/liveness-probe",
 @endpoint_liveness_status_failure=502,
 @endpoint_liveness_status_success=202,
 @endpoint_readiness="/readiness-probe",
 @endpoint_readiness_status_failure=503,
 @endpoint_readiness_status_success=203,
 @endpoint_startup="/startup-probe",
 @endpoint_startup_status_failure=501,
 @endpoint_startup_status_success=201,
 @endpoints_namespace="/application-healthcheck",
 @services={:postgres=>#<Proc:0x0000000103347688 (lambda)>, :redis=>#<Proc:0x0000000103347610 (lambda)>, :rabbit=>#<Proc:0x00000001033475e8 (lambda)>},
 @services_liveness=[:redis],
 @services_readiness=[:postgres, :redis, :rabbit],
 @services_startup=[:postgres]>
```

### Reseting configuration

Also you can reset `OnStrum::Healthcheck` configuration.

```ruby
OnStrum::Healthcheck.reset_configuration!
=> nil
OnStrum::Healthcheck.configuration
=> nil
```

## Integration

Please note, to start using this middleware you should configure `OnStrum::Healthcheck` before and then you should to add `OnStrum::Healthcheck::RackMiddleware` on the top of middlewares list.

### Rack

```ruby
require 'on_strum/healthcheck'

# Configuring OnStrum::Healthcheck with default settings
OnStrum::Healthcheck.configure

Rack::Builder.app do
  use OnStrum::Healthcheck::RackMiddleware
  run YourApplication
end
```

### Roda

```ruby
require 'on_strum/healthcheck'

# Configuring OnStrum::Healthcheck with default settings
OnStrum::Healthcheck.configure

class YourApplication < Roda
  use OnStrum::Healthcheck::RackMiddleware
end
```

### Hanami

```ruby
# config/initializers/on_strum_healthcheck.rb

require 'on_strum/healthcheck'

# Configuring OnStrum::Healthcheck with default settings
OnStrum::Healthcheck.configure
```

```ruby
# config/environment.rb

Hanami.configure do
  middleware.use OnStrum::Healthcheck::RackMiddleware
end
```

### Rails

```ruby
# config/initializers/on_strum_healthcheck.rb

require 'on_strum/healthcheck'

# Configuring OnStrum::Healthcheck with default settings
OnStrum::Healthcheck.configure
```

```ruby
# config/application.rb

class Application < Rails::Application
  config.middleware.use OnStrum::Healthcheck::RackMiddleware
end
```

## Healthcheck endpoint response

Each healthcheck endpoint returns proper HTTP status and body. Determining the response status is based on the general result of service checks (when all are successful the status is successful, at least one failure - the status is failure). The response body represented as JSON document with a structure like in the example below:

```json
{
  "data": {
    "id": "a09efd18-e09f-4207-9a43-b4bf89f76b47",
    "type": "application-startup-healthcheck",
    "attributes": {
        "postgres": true,
        "redis": true,
        "rabbit": true
    }
  }
}
```
