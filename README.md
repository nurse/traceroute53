# Traceroute53

A tool to investigate Route53, ELB, EC2 and Security Groups

[![Gem Version](https://badge.fury.io/rb/traceroute53.svg)](https://badge.fury.io/rb/traceroute53)

## Installation

Add this line to your application's Gemfile:

```ruby
gem install traceroute53
```

## Usage

```
traceroute53 <domain>
```

To pass credentials, [set environment variables](https://docs.aws.amazon.com/sdk-for-ruby/v3/developer-guide/setup-config.html#aws-ruby-sdk-credentials-environment) or speicfy `--profile=PROFILE` option.

### An example for healthy domain

Below is an example for healty domain. It show the ELB has a target instance and it can foward requests to it. Because the security group associated the instance allows LB's security group.

```
% traceroute53 foo-development.example.com
hosted zone: example.com.
dns name: internal-foo-aws-1-123456.us-east-1.elb.amazonaws.com.
load balancer: foo-aws-1 ["sg-0aaaaaaaaaaaaaa1", "sg-ccccccccccccccccc"]
listener[0]: port:443 arn:aws:elasticloadbalancing:us-east-1:567890123456:listener/app/foo-aws-1/7890123456789abc/0cdef01234567789
listener[0]action[0]: forward arn:aws:elasticloadbalancing:us-east-1:567890123456:targetgroup/foo-aws-1/89abcdef01234567
listener[0]action[0]target[0]: i-0cdef0123456789ab:8080 healthy
group_ids[0]: ["sg-09988776655443322", "sg-39393939"]
group_ids[0]sg[0]: sg-09988776655443322
group_ids[0]sg[0]ip[0]: port:8080 ["sg-ccccccccccccccccc"]
group_ids[0]sg[0]ip[1]: port:22 ["sg-05566778899aabbcc", "sg-f8e8d8c8"]
group_ids[0]sg[1]: sg-39393939
group_ids[0]sg[1]ip[0]: port:8080 ["sg-11223344"]
group_ids[0]sg[1]ip[1]: port:nil ["sg-f8f8f8f8"]
group_ids[0]sg[1]ip[2]: port:22 ["sg-33886655"]
```

### An example for unhealthy domain

In this example Route53's hosted zone correctly have dns resource, which has correct dns\_name, listener, target group but its 2nd security group's Permission set is empty.

```
% traceroute53 bar-blah.example.com
hosted zone: example.com.
dns name: internal-bar-blah-aws-tokyo-1-999888333.ap-northeast-1.elb.amazonaws.com.
load balancer: bar-blah-aws-tokyo-1 ["sg-0eeddccbbaa998877", "sg-06665554443332221"]
listener[0]: port:443 arn:aws:elasticloadbalancing:ap-northeast-1:567890123456:listener/app/bar-blah-aws-tokyo-1/ef0123456789abcd/cccaaabbb9996667
listener[0]action[0]: forward arn:aws:elasticloadbalancing:ap-northeast-1:567890123456:targetgroup/bar-blah-atyo-1/fedcba9876543210
listener[0]action[0]target[0]: i-0cc123456789abcd:8080 unhealthy
group_ids[0]: ["sg-c57c55cc", "sg-0336699ccff003366"]
group_ids[0]sg[0]: sg-c57c55cc
group_ids[0]sg[0]ip[0]: port:8080 ["sg-99776655"]
group_ids[0]sg[0]ip[1]: port:nil ["sg-11335577"]
group_ids[0]sg[0]ip[2]: port:22 ["sg-fe87dc65"]
group_ids[0]sg[1]: sg-0336699ccff003366
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake spec` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/nurse/traceroute53.

## License

The gem is available as open source under the terms of the [MIT License](https://opensource.org/licenses/MIT).
