### Organizing security groups

Security Groups provide a modular way to define and compose firewall rules.

A security group is a set of firewall rules. Security groups are attached to
instances at creation time. It is not possible to add a security group to an
existing instance.

Therefore it is important to carefully think about your security groups when
setting up any infrastructure and find a balance between:

* Having one security group per instance and manage firewall rules on a
  per-instance basis
* Having a single security group for all instances and ending up with no
  granularity when it comes to network restrictions.

A common practice is to identify *roles* in your infrastructure. For instance,
an application infrastructure can be composed of:

* application servers
* cache servers
* load balancers
* database servers
* etc, etc.

In this case you could have:

* A "common" security group that defines rules that are common to all
  machines, such as SSH access or internal communication
* One security group per role, defining rules specifically for database
  servers or load balancers.

When setting up a new database server, you would give it the "common" and the
"database" security groups.

It is also a good practice to apply this technique even with small
architectures where a single machine can play all different roles. This
way your infrastructure is ready for growth and allows later separation of
services across different machines.

### Security group features

When adding a rule to a security group, you can set the following properties:

* **Traffic type**: inbound or outbound. By default all outbound traffic is
  permitted, however as soon as you define an outbound rule, outbound traffic
  is only allowed for the defined outbound rules. See [managing outbound
  security rules](/documentation/open-cloud/tutorials/outbound-security-rules)
  for more information.

* **Source type**: this can be a CIDR or a security group. This allows you to
  define internal rules between security groups without the hassle of using IP
  addresses directly.

* **Protocol**: TCP, UDP or ICMP.

* **Start port** and **end port**: this lets you define rules for a specific
  port (set the same port as start and end port) or for a whole range.
