# sentinel-template-terraform

[![Run Sentinel unit tests](https://github.com/hashicorp-services/sentinel-bootstrap-tfe/actions/workflows/sentinel-tests.yml/badge.svg)](https://github.com/hashicorp-services/sentinel-bootstrap-tfe/actions/workflows/sentinel-tests.yml)

Sentinel starter repo with directory structure and Sentinel functions built out intended to be shared directly with customers.

Sentinel CLI Tested: `v0.18.11`

## Why this repository?

Often with TFE customers who are just getting started in TFE, governance is of interest but the focus is elsewhere. The attempted solution here is a quick repo we can drop into any customer environment, wire up to their TFE install, and leave them with a framework to work off of.


Todo:

- Debug param?
- function testing

### Testing this repository

To run cloud specific tests, or develop policies, run the following:

```sh
cd ./policies/aws
sentinel test
sentinel test -run required-tags 
sentinel test -run required-tags -verbose
```

To run all the tests for all the policies, run the following:

```sh
# Without details
sentinel test ./policies/*        
# With details
sentinel test -verbose ./policies/*        
```

## About this Repo

Initial bootstrap to get you going and includes 2 default policies for each cloud.

- Required Tags
- Restrict Instance Size

### `policies` folder

Default policies associated with each of the top 4 clouds, includes tests.

### `policy-sets` folder

3 Policy sets as an example:

- Global - Meant for all Workspaces
- Non-Production - Meant for Non-Production Workspaces
- Production - Meant for Production Workspaces

By default each of these sets connects to all the default policies for all the clouds as `advisory` to not restrict any workflows.

### `functions` folder

Common functions used by the default policies, these are required for the policy sets to operate in TFE.

## Sentinel Policy Writing Flow

At a high level, the following is a good way to break down the process of creating/writing Sentinel Policies:

1. Gather all resources or filter by resource type (_i.e._"aws_s3_bucket") and actions (_i.e._ "create", "update", "no-op").
2. Evaluate resource attribute values by rule (inline) or function (from custom import).
3. Return violation summary in main.

>Treat a policy like a funnel by filtering out the things you don't care about and only focusing on the things that are important to evaluate. The single most important thing is ensuring that a policy is easy to read, understand, and doesn't require context switching when things break.

## Sentinel Best Practices

**Tests**

- Create all tests in a `policies/<cloud>/test/<policy name>/` folder.
- Tests should prefix with "test-"
- Mocks should prefix with "mock-"

At the **very** least 1 positive and 1 negative test.

**Clear Naming**

- enforce
  - cause (something) to happen by necessity or force.
  - enforce a pattern or strategy
- require
  - need for a particular purpose.
  - require some property to be set or not set
- restrict
  - put a limit on; keep under control.
  - restrict what values a property can or can not be set to

**Format**

Sentinel CLI doesn't have a recursive format, this will work:

```
find . -name "*.sentinel" -type f | xargs sentinel fmt
```

## References

- [TFE Sentinel Documentation](https://www.terraform.io/docs/cloud/sentinel/index.html)
- [Sentinel Documentation](https://docs.hashicorp.com/sentinel)
- [Examples](https://github.com/hashicorp/terraform-guides/tree/master/governance/third-generation)
- [VS Code Plugin](https://marketplace.visualstudio.com/items?itemName=HashiCorp.sentinel)
