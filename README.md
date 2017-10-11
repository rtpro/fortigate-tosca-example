# Fortigate Sample TOSCA Template

This repo contains an example TOSCA template that deploys and configures a [Fortigate VNF](https://www.fortinet.com/products/virtualized-next-generation-firewall.html) in Openstack.  It presumes that the orchestrator is Apache [ARIA](http://ariatosca.incubator.apache.org/), mainly because it utilizes a TOSCA extension that permits the use [Cloudify](http://cloudify.co) plugins.  The plugins in question are the Openstack plugin (https://github.com/cloudify-cosmo/cloudify-openstack-plugin), and the utilities plugin (https://github.com/cloudify-incubator/cloudify-utilities-plugin).

## Functional Description
The [fortigate-vnf-baseline-tosca.yaml](fortigate-vnf-baseline-tosca.yaml) file creates a publicly accessible network and a private network, with a Fortigate VNF in between to control access.  The Cloudify [Openstack plugin](https://github.com/cloudify-cosmo/cloudify-openstack-plugin) is used to model the Openstack network and [Fortigate VNF](https://www.fortinet.com/products/virtualized-next-generation-firewall.html).  The terminal functionality provided by the [Cloudify utilities plugin](https://github.com/cloudify-incubator/cloudify-utilities-plugin) is used to configure the VNF after it has started.

## Usage
Prior to using the template, some significant prequesites must be satisfied:
* Have a Python 2.7 environment configured including the latest Pip and Virtualenv
* Install Apache ARIA
* Install the Cloudify Openstack plugin
* Install the Cloudify incubator utilities plugin
* Gain API access to an Openstack cloud
* Gain access to a Fortigate image for Openstack
* Configure the template
* Install and instantiate the template

### Install Apache ARIA
This is covered by ARIA [documentation](http://ariatosca.incubator.apache.org/getting-started/)

### Install the Cloudify Openstack plugin
* Enter your ARIA virtualenv: `source <virtual-env-dir>/bin/activate`
* If [Wagon](https://github.com/cloudify-cosmo/wagon) isn't installed, install it: `pip install wagon`.
* Next clone the Openstack plugin: `git clone -b 2.0.1 https://github.com/cloudify-cosmo/cloudify-openstack-plugin`
* Then create a wagon from the plugin source: `wagon create cloudify-openstack-plugin`.
* Then install the plugin in ARIA: `aria plugins install cloudify_openstack_plugin-2.0.1-py27-none-linux_x86_64.wgn`

### Install the Cloudify incubator utilities plugin
* Enter your ARIA virtualenv: `source <virtual-env-dir>/bin/activate`
* If [Wagon](https://github.com/cloudify-cosmo/wagon) isn't installed, install it: `pip install wagon`.
* Next clone the utilities plugin: `git clone -b 1.3.0 https://github.com/cloudify-incubator/cloudify-utilities-plugin`.
* Then create a wagon from the plugin source: `wagon create cloudify-utilities-plugin`.
* Then install the plugin in ARIA: `aria plugins install `cloudify_utilities_plugin-1.3.0-py27-none-linux_x86_64.wgn`.

### Configure the template
Edit the following values in the [fortigate-vnf-baseline-tosca.yaml](fortigate-vnf-baseline-tosca.yaml) file:
* In the `openstack_config` macro:
** username: your Openstack user name
** password: your Openstack password
** tenant_name: your Openstack tenant name
** auth_url: the Openstack Keystone URL
* In the `fortigate_image_url` input definition:
** edit the `default` value and put in a URL that points to the Fortigate VNF `img` file.

### Install and instantiate the template
* `aria service-templates store fortigate-vnf-baseline-tosca.yaml <your-template-name>`
* `aria services create -t <your-template-name> <your-service-name>`
* `aria executions start -s <your-service-name> install


