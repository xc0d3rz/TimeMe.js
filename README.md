<h3>What is TimeMe.js?</h3>
TimeMe.js is a JavaScript library that accurately tracks how long users interact with a web page.
TimeMe.js disregards time spent on a web page if the user minimizes the browser or 
switches to a different tab.  This means a more accurate reflection of actual 'interaction' time by 
a user is being collected.  Additionally, TimeMe.js disregards 'idle' time outs.  If the user goes 
idle (no page mouse movement, no page keyboard input) for a customizable period of time,
then TimeMe.js will automatically ignore this time.  This means no time will be reported
where a web page is open but the user isn't actually interacting with it (such as when
they temporarily leave the computer).  These components put together create a much more accurate
representation of how long users are actually using a web page.

Furthermore - TimeMe tracks time usage across multiple pages.  This is particularly 
useful when running a single page web application. You can get a list of all aggregate 
times spent on all pages from TimeMe.js.

<h3>Demo</h3>
You can see a demo of a timer using TimeMe.js 
<a target="_blank" href="http://jasonzissman.com/timeme/index.html">here</a>.

<h3>Where do I get TimeMe.js?</h3>
You can use Bower to install TimeMe (see next steps) and its dependencies.
Alternatively, you can download the most recent copy at <a href="https://github.com/jasonzissman/TimeMe.js">the TimeMe Github project</a>.
Notice you will also need a copy of <a href="https://github.com/serkanyersen/ifvisible.js">
Serkanyersen's ifvisible.js project</a>. The ifvisible.js library is REQUIRED to allow
TimeMe.js to work.
<h3>How do I use TimeMe.js?</h3>
First, obtain a copy of timeme.js and ifvisible.js.  You can get both by installing TimeMe.js via Bower: <br/><br/>
<div class="code-block"><pre><code>bower install timeme.js</pre></code></div><br/><br/>
Then, simply include the following lines of code in your page's head element: <br/><br/>

<div class="code-block">
<pre>
<code>
&lt;script src="ifvisible.js">&lt;/script&gt;
&lt;script src="timeme.js"></script">
&lt;script type="text/javascript">
 TimeMe.setIdleDurationInSeconds(30);
 TimeMe.initialize();        
&lt;/script&gt;</code>
</pre>
</div>
<br/>

This code both imports the TimeMe.js library and initializes it. Notice that this code sets the idle duration to 30 seconds, which means 30 seconds of user inactivity (no mouse or keyboard usage on the page) will stop the timer. Also, we used default timer ID (**default**).
<br>
Once imported and initialized, we can call the various methods made available
by TimeMe.js.  See the <a href="#API">API documentation</a> below for
a complete breakdown of all of the available functionality.  The most basic
feature is to retrieve the time spent by the user on the default timer:<br/><br/> 
<div class="code-block">
<pre><code>
 var timeSpentOnPage = TimeMe.getDefaultTimer();
</code>
</pre>
</div>
<br>
In most cases you will want to store the time spent on a page for analytic purposes. You will therefore need to send the time spent on a page to the server at some point! When is the best time to do this? You can hook into the window.onbeforeunload event to do so. In most browsers this method is fired during a page's shut-down routine. Notice below that we use a synchronous request (not the usual asynchronous request) to guarantee the request to our server arrives before the page closes:

<div class="code-block">
<pre><code>
window.onbeforeunload = function (event) {
    xmlhttp=new XMLHttpRequest();
    xmlhttp.open("POST","ENTER_URL_HERE",false);
    xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
    var timeSpentOnPage = TimeMe.getDefaultTimer();
    xmlhttp.send(timeSpentOnPage);
};
</code></pre>
</div><br/>
Using 'onbeforeunload' is by no means a requirement. You can hook into any other event or logical point in your application to send the time spent information to the server. 
<br>
In a traditional if you want another timer on same page without stop or rest the default timer:
<div class="code-block">
<pre><code>
var TimerID = "xC0d3rZ"
TimeMe.initialize(TimerID);
var timeSpend = TimeMe.getTimer(TimerID);
</code></pre>
</div>
<br />
> Such easy !

All page times are tracked in TimeMe.js, so you can review total aggregate timers on each page for a particular user's session:
<div class="code-block">
<pre><code>
var timersReport = TimeMe.getAll();
</code></pre>
</div>
<br />
<h3>API</h3>

<div class="code-block">
<pre><code>
TimeMe.setDefaultID(newDefaultID);
</code></pre>
</div>
Sets the Default Timer ID.
<br/><br/>
</div><br/>			
<div class="code-block">
<pre><code>TimeMe.setIdleDurationInSeconds(durationInSeconds);</code></pre>
Sets the time (in seconds) that a user is idle before the timer is
turned off.  Set this value to -1 to disable idle time outs.
<br/><br/>
</div><br/>		
<div class="code-block">
<pre><code>TimeMe.initialize(TimerID);</code></pre>
Initializes the timer.  Should only be called when first importing the
library and beginning to time page usage.
If you set TimerID will start new Timer with The stetted vaule as ID,unless will start the Default timer (Time Spent on Page) 
<br/><br/>
</div><br/>				
<div class="code-block">
<pre><code>var timeInSeconds = TimeMe.getDefaultTimer();</code></pre>
Retrieves the time spent (in seconds) for Default ID.
<br/><br/>
</div><br/>
<div class="code-block">
<pre><code>var timeInSeconds = TimeMe.getTimer(TimerID);</code></pre>
Retrieves the time spent (in seconds) for the indicated Timer ID.
<br/><br/>
</div><br/>	
<div class="code-block">
<pre><code>var timeSpentInfo = TimeMe.getAll();</code></pre>
Retrieves the time spent on all timers that have been recorded using TimeMe.js.
Notice this only works for Single Page Applications (SPAs) where TimeMe.js is
only initialized once.
<br/><br/>
</div><br/>	
<div class="code-block">
<pre><code>TimeMe.startTimer(TimerID);</code></pre>
Manually starts the timer for the indicated Timer ID..  Notice this only works if the
timer is currently stopped.
<br/><br/>
</div><br/>	
<div class="code-block">
<pre><code>TimeMe.stopTimer(TimerID);</code></pre>
Manually stops the timer for the indicated Timer ID.  Notice this only works if the timer is currently running.
<br/><br/>
</div><br/>
<div class="code-block">
<pre><code>TimeMe.resetRecordedTimer(TimerID);</code></pre>
Clears the recorded time for the indicated TimerID.
<br/><br/>
</div><br/>	
<div class="code-block">
<pre><code>TimeMe.TimeMe.resetAllRecordedTimers();</code></pre>
Clears all recorded timers.
<br/><br/>
</div><br/>				
</div>		



