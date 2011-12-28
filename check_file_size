#!/usr/bin/perl

# Written by Chris McDermott, AppliedTrust, chris@appliedtrust.com
# Last modified 6/23/2011

use strict;

die("usage: check_file_size <FILE> <WARN size in bytes> <CRIT size in bytes>") unless (@ARGV == 3);

my ($thefile, $warn, $crit) = @ARGV;
if (`! [ -f $thefile ]`) {
        print "$thefile does not exist, or is not a regular file.  Please specify a complete path and filename such as /usr/local/na
gios/var/serviceperf.log\n";
        exit 1;
}
elsif ($warn >= $crit) {
        print "warning threshold should be smaller than critical threshold!\n";
        exit 1;
}

my $shortname = $thefile;
if ($thefile =~ /([^\/]+)$/) {
        $shortname = $1;
}


my $lsOutput = `ls -l $thefile`;
my @line = (split/\s/, $lsOutput);

#foreach (@line) {
#       print "$_\n";
#}

my $size = $line[4];

if ($size >= $crit) {
        print "FILESIZE CRITICAL - $thefile is $size bytes |${shortname}_size=$size\n";
        exit 2;
}

elsif ($size >= $warn) {
        print "FILESIZE WARNING - $thefile is $size bytes |${shortname}_size=$size\n";
        exit 1;
}

else {
        print "FILESIZE OK - $thefile is $size bytes |${shortname}_size=$size\n";
}
