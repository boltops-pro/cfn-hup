# cfn-hup configset

![CodeBuild](https://codebuild.us-west-2.amazonaws.com/badges?uuid=eyJlbmNyeXB0ZWREYXRhIjoiV3JyMHYvZHVPNlNORnloUVE1YzlNTWJkZUJmNmVtaXdvZ1pKV1U4SUUrcU9VcTVZdm5qbVA2cWdIdUZ0VEIraTVyaFpUajBpK2NvZmNFeTRWaEI1TnI0PSIsIml2UGFyYW1ldGVyU3BlYyI6IjZFRnpCNXh4ZEY3L2dqS00iLCJtYXRlcmlhbFNldFNlcmlhbCI6MX0%3D&branch=master)

[![BoltOps Badge](https://img.boltops.com/boltops/badges/boltops-badge.png)](https://www.boltops.com)

## Why?

Configsets are essentially configuration management. Use configsets to configure and update your EC2 instances automatically. Lono allows you to use configsets in a reusable way.

Configsets work with [AWS::CloudFormation::Init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html) and [cfn-init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-init.html). Configsets do not magically get applied after being added to the CloudFormation template though.  The `cfn-init` script must be called to apply the configset.

Usually the `cfn-init` script is called in the UserData script. This ensures configsets are applied when instances are launched.

Additionally, the [cfn-hup](https://github.com/boltopspro/cfn-hup) script can be set up to apply configsets continuously.

This configset configures and runs the cfn-hup daemon. The `cfn-auto-reloader.conf` is configured to detect changes every 1 minute.  When the stack [Metatdata AWS::CloudFormation::Init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html) changes, cfn-hup detects this and updates the EC2 instance. This ensures that the EC2 instance will always be up-to-date.

The cfn-hup configset should usually be one of the first things to be setup.

## What are lono configsets?

Lono configsets allow CloudFormation [cfn-init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/cfn-init.html) [configsets](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html) that are typically embedded in the template to be reusable.  More info: [Lono Configsets docs](https://lono.cloud/docs/configsets/).

## Usage

Use `configset` to enable the configset for the blueprint.  Example:

configs/demo/configsets/base.rb:

```ruby
configset("cfn-hup", resource: "Instance")
```

This adds the configset to the `resource` with the logical id `Instance` in your CloudFormation template.  The configset is added to the [Resources[].Metadata.AWS::CloudFormation::Init](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/aws-resource-init.html) attribute of the `Instance` resource.

Here's an example adding to a `LaunchConfig` resource:

```ruby
configset("cfn-hup", resource: "LaunchConfig")
```

Here's an example adding to a `LaunchTemplate` resource:

```ruby
configset("cfn-hup", resource: "LaunchTemplate")
```
