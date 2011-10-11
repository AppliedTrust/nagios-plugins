This repository contains Nagios plugins developed by AppliedTrust.

------------------------------------------------------------------
We offer no warantee or guarantee - use this code at your own risk!
All code is Copyright (C) 2011, Applied Trust Engineering, Inc.
------------------------------------------------------------------

NAGIOS CHECK SCRIPT DESCRIPTIONS:

check_aws_rds: A simple Nagios plugin for monitoring Amazon AWS's Relational Database Service (RDS).

check_phpapc: A Nagios plugin for monitoring the APC opcode cache for PHP. The apc.php status script (which ships with php-apc) must be installed on the system to be monitoried and made accessible via HTTP/HTTPS.

check_aws_cloudwatch: A simple Nagios plugin for monitoring Amazon AWS EC2 metrics via CloudWatch.

check_http_md5: A simple Nagios plugin to grab a webpage and use an MD5 hash to check if the content has changed.

check_selenium: A Nagios plugin to run remote Selenium tests via PHPUnit.

create_ebs_snapshot: A Nagios plugin to snapshot an AWS EBS volume.

prune_ebs_snapshots: A Nagios plugin to prune old AWS EBS volume snapshots created by create_ebs_snapshot.

check_pagespeed: A Nagios plugin to check the Google PageSpeed score for a site.

