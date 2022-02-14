# Appsbroker Project Chart

The Appsbroker Project Chart is designed to create a new Project, set up this project managed by Config Connector and create a VPC and private subnet in the new Project.

The idea is to create a repeatable pattern in which Projects can be quickly created and bootstrapped with little input/overhead.

## Installation

```bash
helm repo add appsbroker https://lewisstrong-appsbroker.github.io/helm-charts/
```

## Values

| Value                    | Description                                                                           | Type   | 
| -----------              | -----------                                                                           | ---    |
| mgmtAccount              | The Project ID of the MGMT Config Connector cluster which will manage the new Project | string |
| project.name             | The name of the new Project to be created                                             | string |
| project.customer         | The customer which this project will be associated with                               | string |
| project.environment      | The environment type of which this project will be                                    | string |
| project.parentFolderID   | The Folder ID of the Folder which this new Project will be created in                 | string |
| project.billingAccountID | The Billing Account ID which this Project will be associated with                     | string |
| project.organizationID   | The Organization ID which this Project will reside within                             | string |
| subnet.create            | An object map of the Subnets to create consisting of CIDR and Region                  | map    |
| iam.viewer               | List of users to be associated with the project/viewer role on the new project        | list   |

## Subnet

By default the chart will set up a Private Subnet in `europe-west2` with a CIDR of `192.168.0.0/20`. You can override this by specifying your own requirements within the `subnet.create` map with multiple subnets as per example below:

```yaml
subnet:
  create:
    private:
      cidr_range: 192.168.0.0/20
      region: europe-west2
    public:
      cidr_range: 192.168.16.0/20
      region: europe-west2
    mgmt:
      cidr_range: 192.168.32.0/20
      region: europe-west2
```

## IAM

You can specify a list of users who will be granted the project/viewer role upon Project creation as per example below:

```yaml
iam:
  viewer:
    - user:user.surname@company.com
    - user:user2.surname@company.com
    - user:user3.surname@company.com
```