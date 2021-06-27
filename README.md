# check_aix_cpu
nagios check for AIX CPU


# Requirements
perl, ssh  on nagios server

# Configuration

This script is executed remotely on a monitored system by the NRPE or check_by_ssh  methods available in nagios.

If you hare using the check_by_ssh method, you will need a section in the services.cfg file on the nagios server that looks similar to the following.
This assumes that you already have ssh key pairs configured.
```
    define service {
       use                             generic-service
       hostgroup_name                  all_aix
       service_description             AIX CPU
       check_command                   check_by_ssh!/usr/local/nagios/libexec/check_aix_cpu
       }
```

Alternatively, if you are using the NRPE method, you should have a section similar to the following in the services.cfg file:
```
    define service{
       use                             generic-service
       hostgroup_name                  all_aix
       service_description             AIX CPU
       check_command                   check_nrpe!check_aix_cpu -t 30
       notification_options            c,r                     ; Send notifications about critical and recovery events
       }
```

If you are using the NRPE method, you will also need a command definition similar to the following on each monitored host in the /usr/local/nagios/nrpe/nrpe.cfg file:
```
    command[check_aix_cpu]=/usr/local/nagios/libexec/check_aix_cpu
```

Please note that the default thresholds for alerting are as follows:
```
    WARN on iowait cpu utilization 50% or more
    CRIT on iowait cpu utilization 80% or more
    WARN on user   cpu utilization 75% or more
    CRIT on user   cpu utilization 90% or more
    WARN on sys    cpu utilization 75% or more
    CRIT on sys    cpu utilization 90% or more
    WARN on idle   cpu utilization 10% or less
    CRIT on idle   cpu utilization 5%  or less
    WARN on entitled cpu utilization 500% or more
    CRIT on entitled cpu utilization 800% or more
```
