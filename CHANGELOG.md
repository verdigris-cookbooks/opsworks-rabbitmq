# v0.2

* Removed `opsworks_rabbitmq::dns` recipe (functionality now provided by
  [opsworks_route53](https://github.com/verdigris-cookbooks/opsworks_route53)
  cookbook)
* Fixed **service cannot be found** error in `opsworks_rabbitmq::configure`
  recipe

# v0.1.1

* Fixed a minor bug with AWS Route53 zone ID attribute not being resolved
correctly

# v0.1.0

* Initial release based on original RabbitMQ cookbook
