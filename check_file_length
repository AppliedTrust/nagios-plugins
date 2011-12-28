#!/usr/bin/perl

use FileHandle;

die("usage: check_file_length <SHORTNAME> <FILE> <WARN lines/min> <CRIT lines/min>") unless (@ARGV == 4);

($shortname, $thefile, $warn, $crit) = @ARGV;

$servicename = "FileLen-".$shortname;

$ECFFILE = "/usr/local/nagios/var/rw/nagios.cmd";
$DEBUG = 0;
$DEBUGLOG = "/usr/local/nagios/var/tmp/file_length_debug.log";


#######################

$fileshort = $thefile;
$fileshort =~ s/\//_/g;

$DELTA = "/usr/local/nagios/var/tmp/file_length_delta_" . $fileshort;

if (!-f $thefile) {
	print($servicename . " CRITICAL - Could not find the file - ".$thefile."\n");
	&debug("CRITICAL - Could not find the file - ".$thefile);
	exit(2);
}

if (open(DELTA, $DELTA)) {
	chomp($old_linecount = <DELTA>);
	chomp($old_timestamp = <DELTA>);
	close(DELTA);
} else {
	chomp($old_linecount = 0);
}

### this is it:
@result = `/usr/bin/wc -l $thefile`;
$nowtimestamp = time();

$theline = $result[0];

if ($theline =~ /(\d+)\s+/) {
	$currentlinecount = $1;
} else {
	print($servicename . " CRITICAL - failed to get file length (parse error): $!\n");
	&debug("CRITICAL - failed to get file length (parse error): $!");
	exit(2);
}

if (open(DELTA, ">$DELTA")) {
	if ($currentlinecount < $old_linecount ) {
		print($servicename . " WARNING - File/log must have wrapped: $thefile\n");
		&debug("WARNING - File/log must have wrapped: $thefile");
		exit(1);
	}

	$linedelta = ($currentlinecount - $old_linecount);
	$timedelta = ($nowtimestamp - $old_timestamp);

		# this is meant to run NO MORE THAN ONCE A MINUTE
		if ($timedelta <= 30) { $timedelta = 30; }  # divide by zero protection AND don't want unrealistic # of lines ever

	$theLPM = ($linedelta / ($timedelta / 60)); ## lines per minute

	&debug("OK - ".$servicename.": new linecount: ".$currentlinecount." (delta=".$theLPM." timedelta=".$timedelta.")");

	if ( $theLPM >= $crit) {
		printf("%s CRITICAL - %d lines/min|%s_lpm=%d\n",$servicename,$theLPM,$servicename,$theLPM);
		print DELTA qq/$currentlinecount\n/;
		print DELTA qq/$nowtimestamp\n/;
		close DELTA;
		exit(2);
	} elsif ( $theLPM >= $warn) {
		printf("%s WARNING - %d lines/min|%s_lpm=%d\n",$servicename,$theLPM,$servicename,$theLPM);
		print DELTA qq/$currentlinecount\n/;
		print DELTA qq/$nowtimestamp\n/;
		close DELTA;
		exit(1);
	} else {
		printf("%s OK - %d lines/min|%s_lpm=%d\n",$servicename,$theLPM,$servicename,$theLPM);
		print DELTA qq/$currentlinecount\n/;
		print DELTA qq/$nowtimestamp\n/;
		close DELTA;
		exit(0);
	}
	

} else {
	print($servicename . " CRITICAL - Cant overwrite delta file: $DELTA\n");
	&debug("ERROR: Cant overwrite delta file: $DELTA");
	exit(2);
}

exit 2;


sub debug {
	my ($message) = @_;
	if ($DEBUG) {
		open(LF, ">>$DEBUGLOG") || die("couldnt open debug log");
		printf LF "debug: " . $message . "\n";
		close(LF);
	}
}


