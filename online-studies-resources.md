# Online Studies Resources

Request the login info from Mick to access the lab's [mTurk requester](https://www.mturk.com/) account, the [developer Sandbox](https://requester.mturk.com/developer/sandbox) account, & the [Amazon Web Services](https://aws.amazon.com/s3/) (S3) account.

Tutorials:

[mTurk tutorial](https://bradylab.ucsd.edu/ttt/) by the Brady Lab

[mTurk guide](https://docs.google.com/document/d/17HgAWTeI6qZl8vzXzXXjQFKFTDcrR_ggbuP0eYqK2EE/edit) by Martin Hebart

-----

Prolific:
https://www.prolific.co

On top of improved data quality, there are lots of "quality of life" improvements over mturk, such as

* A much simpler interface
* The ability to message anyone who starts your study
* The ability to bonus or approve anyone who starts your study (no more compensation HITs!)
* Lower fees than mturk (unless you used batching, and assuming no currency exchange fees)
* It keeps track of actual pay per hour as participants finish
* The study completion percentage updates more quickly
* A status indicator for each participant (finished, in progress or cancelled)
* An estimate of the size of the eligible participant pool given your qualifications/restrictions

To successfully interface your study with Prolific, you'll need to do a few things:

* For Prolific (unlike MTurk), participants complete your study directly from your URL you provide (instead within the MTurk frame/interface)
* So importantly: You *NEED* to put a URL specific to your study here (I put mine at the top of my script). You'll get this for each study from the Prolific experiment creation page.
* And you *NEED* to redirect them after the study is over (or at least give them the link explicitly)

Other info:

* Balance: Make sure there's enough money in our account (top right) before running! If you need more money, message Mick. He'll email the admins to add it.
* Payment: we must pay $10/hour. Calculate your study time fairly.
* You can extract the Prolific PID from the URL. But you need to do "Show Advanced" and check the box:
  * Would you like us to include special parameters such as PARTICIPANT_ID in your study url?
* To preview your study before publishing, there's a Preview button at the bottom of the Study screen
* I make mine accessible only by Desktop
* You can apply prescreening for various things. I do:
  * Nationality (I do US, but you may want UK too)
  * Approval rate (I do 85 and up, but the data is so good you really don't need this)
  * Number of previous submissions (I do 50 and up, but the data is so good you really don't need this)

-----

In the HTML you need a div something like the below:

```HTML
<div class="doneTextStyle" id="redirectToProlific">
  <p>Your browser should automatically redirect you to Prolific's webpage where you will be given a completion code.</p>
  <p>Redirecting in <span id="countDown"><b></b></span> seconds.</p>
  <p>If you're not automatically redirected in a few seconds, please click the below link, or copy and paste the address into your web browser:
  <br>
  <b><a id="urlProlific" href=""></a></b>
</div>
```

In your javascript:

```JavaScript
////////////////////////////
////////////////////////////
// redirect URL for the Prolific study

// will be unique to each study
var urlProlific = "https://app.prolific.co/submissions/complete?cc=3FD109C6";
```

How to get the Prolific info from the URL:

```JavaScript
// get prolific info
function getProlificInfo() {
  let urlParams = new URLSearchParams(window.location.search);
  prolificPID = urlParams.get("PROLIFIC_PID");
  studyID = urlParams.get("STUDY_ID");
  sessionID = urlParams.get("SESSION_ID");
  if (prolificPID == null) {
    prolificPID = 'NO-SUBJ-ID';
  }
  return [prolificPID, studyID, sessionID];
}
```

General Prolific redirect functions. You want these in your javascript:

```JavaScript
var finalCountDownClock = 3;

function redirectToProlific() {
  // set url
  $('#urlProlific').text(urlProlific);
  $('#urlProlific').attr("href", urlProlific);
  // hide
  $('#submitText').hide();
  // show
  $('#redirectToProlific').show();
  // countdown
  $('#countDown').text((finalCountDownClock).toString());
  redirectTimer = setInterval(countDown, 1000);
}

// prolific redirect countdown
function countDown () {
  finalCountDownClock--;
  if (finalCountDownClock == 0) {
    // clear countdown
    clearInterval(redirectTimer);
    // redirect
    window.location = urlProlific;
  } else {
    $('#countDown').text((finalCountDownClock).toString());
  }
}
```

-----

How to use `php` to record data:

Put this in your javascript. (`responses` is an object holding the data; `subjectID` is the subject user id; `outDir` is the folder where to store the data):

```JavaScript
// get data string from response array
var dataString = JSON.stringify(responses);
// post response to server
$.post("logTrial.py", {
  subjectID: subjectID,
  dataString: dataString,
  outDir: outDir
});
```

And this should be a file in your experiment folder (*make sure it has appropriate permissions!*). Called e.g. `logTrial.py`:

```python
#!/usr/bin/python

from datetime import datetime
import sys, cgi, cgitb, os, errno

# log any errors in /pylogs/
cgitb.enable(display=0, logdir='pylogs')

# get form data from JavaScript
formdata = cgi.FieldStorage()

# get subjectID and dataString (JSON stringified array)
subjectID = formdata.getvalue('subjectID', 'subjectID_NULL')
dataString = formdata.getvalue('dataString', 'dataString_NULL')
outDir = formdata.getvalue('outDir', '../data') # 'data' is the default directory

# make directory (and catch the error if it already exists; and throw the error if there is another issue besides it not existing, e.g. file permissions)
try:
    os.makedirs(outDir)
except OSError as e:
    if e.errno != errno.EEXIST:
        raise

# get string of current date and time
now = str(datetime.now())[:19].replace(':','-').replace(' ','_')

# optionally, could make it so that if there were no valid subject id, include date/time, but not otherwise

# write file in /data/ as 'A989SDF67SDFA5_2018-07-09_11-56-41.txt
outputFilename = outDir + '/' + subjectID + '_' + now + '.txt'
with open(outputFilename, 'w') as outputFile:
   outputFile.write(dataString)

# print header and 'Done.' message
sys.stdout.write('Content-type: text/plain; charset=UTF-8\n\n')
sys.stdout.write('Done.')
```
