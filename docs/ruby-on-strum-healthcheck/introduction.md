# ![OnStrum::Healthcheck - Simple configurable application healthcheck rack middleware](../assets/images/on_strum_logo.png)

[![Maintainability](https://api.codeclimate.com/v1/badges/b4dc21883d489d67fbef/maintainability)](https://codeclimate.com/github/on-strum/ruby-on-strum-healthcheck/maintainability)
[![Test Coverage](https://api.codeclimate.com/v1/badges/b4dc21883d489d67fbef/test_coverage)](https://codeclimate.com/github/on-strum/ruby-on-strum-healthcheck/test_coverage)
[![CircleCI](https://circleci.com/gh/on-strum/ruby-on-strum-healthcheck/tree/master.svg?style=svg)](https://circleci.com/gh/on-strum/ruby-on-strum-healthcheck/tree/master)
[![Gem Version](https://badge.fury.io/rb/on_strum-healthcheck.svg)](https://badge.fury.io/rb/on_strum-healthcheck)
[![Downloads](https://img.shields.io/gem/dt/on_strum-healthcheck.svg?colorA=004d99&colorB=0073e6)](https://rubygems.org/gems/on_strum-healthcheck)
[![GitHub](https://img.shields.io/github/license/on-strum/ruby-on-strum-healthcheck)](LICENSE.txt)
[![Contributor Covenant](https://img.shields.io/badge/Contributor%20Covenant-v1.4%20adopted-ff69b4.svg)](CODE_OF_CONDUCT.md)

Simple configurable application healthcheck rack middleware. This middleware allows you to embed healthcheck endpoints into your rack based application to perform healthcheck probes. Make your application compatible with [Docker](https://docs.docker.com/reference/dockerfile/#healthcheck)/[Kubernetes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/#define-a-liveness-http-request) healthchecks in a seconds.

## Features

- Built-in default configuration
- Configurable services for startup/liveness/readiness probes
- Configurable root endpoints namespace
- Configurable startup/liveness/readiness probes endpoints
- Configurable successful/failure response statuses

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).

## Versioning

`ruby-on-strum-healthcheck` uses [Semantic Versioning 2.0.0](https://semver.org)
