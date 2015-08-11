#!/usr/bin/python
import argparse, json, subprocess, requests

def main():
        parser = argparse.ArgumentParser()
        parser.add_argument('-U', '--url', help='Jenkins base URL, including basic auth', required=True)
        parser.add_argument('-j', '--jobname', help='Name of the Jenkins job', required=True)
        args = parser.parse_args()

	url = args.url+"/job/"+args.jobname+"/lastBuild/api/json"
        try:
		r = requests.get(url)
		job_data = json.loads(r.text)
        except:
                print "CRITICAL Unable to fetch job status for %s" % args.jobname
                exit(2)

	if (job_data['result'] == 'SUCCESS'):
          print "OK Last %s build successful" % args.jobname
          exit(0)

	elif ((job_data['result'] == None) and ( job_data['building'] == True)):
	  # someday add a check for max runtime
          print "OK Job %s is building now" % args.jobname
          exit(0)

        else:
          print "CRITICAL Last %s build not successful" % args.jobname
          exit(2)

if __name__ == "__main__":
    main()
