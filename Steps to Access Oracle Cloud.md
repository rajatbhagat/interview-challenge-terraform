# Steps to Access Guest OCI Account on mythicsdemo4 tenancy

## Login to OCI Console

1. Vist URL : `https://www.cloud.oracle.com`
1. Enter tenancy name as `mythicsdemo4`
1. Username: `lab_guest`
1. Login should be successful and you should be shown the OCI Console Dashboard

## Setup oci-cli for the mythicsdemo4 tenancy

1. Install oci-cli

    ```commandline
    pip install oci
    pip install oci-cli
    ```
1. Run the command

    ```commandline
    oci os ns get
    ```
    You will have the following prompts. Answer them as specified:

    ```
    ERROR: Could not find config file
    Do you want to create a new config file? [Y/n]: y
    Do you want to create a new config by logging in through a browser? [Y/n] : y
    Enter a region by index or name : 37
    ```
    A browser window should open.

    Enter the tenancy name : `mythicsdemo4`

    Login using the above mentioned credentials.

    The terminal should proceed in creating the OCI config.
    Verify  the config by running the command again
    ```
    oci os ns get

    oci network vcn create --compartment-id ocid1.compartment.oc1..aaaaaaaau2d5tlzxsvkpz3iuzhuo4uemcn7kzmkv6fhjqbpxdzpg5ijz4tqq --cidr-block 198.16.16.0/24
    ```

1. If you already have an oci config setup, then add the below config
    ```
    [DEMO4]
    user=ocid1.user.oc1..aaaaaaaa7zzayonebm5ekziewzdilwrtbfhjqh7g5ogzzv2xe6acpjkqibma
    fingerprint=d0:81:a6:85:2b:e8:38:67:25:80:9c:d8:9a:e6:bc:2c
    tenancy=ocid1.tenancy.oc1..aaaaaaaapgohxfz7epi6c3xs4vfn5pu6ej3hrjjn2zxmk5q5zsfhznq7tqeq
    region=us-ashburn-1
    key_file=<path_to_private_key_file>
    ```

    The private key file for the same would be sent to you.


## Run the following sample terraform script

```terraform
terraform {
  required_providers {
    oci = {
      source = "hashicorp/oci"
    }
  }
}

provider "oci" {
    region = "us-ashburn-1" 
    config_file_profile = <"DEFAULT" | "DEMO4">
}



resource "oci_core_vcn" "internal" {
  dns_label      = "internal"
  cidr_block     = "172.16.0.0/20"
  compartment_id = "ocid1.compartment.oc1..aaaaaaaau2d5tlzxsvkpz3iuzhuo4uemcn7kzmkv6fhjqbpxdzpg5ijz4tqq"
  display_name   = "Sample VCN"
}

```

