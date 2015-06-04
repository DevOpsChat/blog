---
title: Preventing Duplicate Cron Commands
categories:
  - cron
  - tutorial
---
<p>[=Cron is a pretty fantastic tool in Nix systems for running commands at a timed interval. There are some pretty fantastic tools out there that help you with your cron setup. For example:
</p>
<ul>
	<li><span></span><a href="http://www.cronchecker.net/" target="_blank">Cron Checker</a> - Returns written English of your cron command.</li>
	<li><a href="http://abunchofutils.com/u/computing/cron-format-helper/" target="_blank">Cron Format Helper</a></li>
	<li><a href="http://crontab-generator.org/" target="_blank">http://crontab-generator.org/</a></li>
</ul>
<p>
	There are many others, a simple google search will return lot's of information. =]</p>
<p>
	Now, I borked one of my servers last night by having too many of my commands take longer than expected. So when the next cron command was triggered, I'd now have two commands competing against each other (possibly collecting the same data).
</p>
<p>
	Flock to the rescue (No, not the 
	<a href="http://flock.fabric.io/" target="_blank">Twitter flock</a>)! Flock allows you to quite simply, <a href="http://linux.die.net/man/1/flock" target="_blank">manage locks from shell scripts</a><a href="http://linux.die.net/man/1/flock"></a>.
</p>
<p>
	In my cron I have the following:
</p>
<pre class="prettyprint lang-javascript">*/10 * * * * /usr/bin/flock -n /tmp/my.lockfile php /path/to/my/command
</pre>
<p>
	If we run that command through the Cron Checker, linked to above, it tells us:
</p>
<p>
	```The command <code>/usr/bin/flock -n /tmp/my.lockfile php /path/to/my/command</code> will execute every 10th minute of every hour every day.```
</p><p>What it isn't telling us is that a lock file is created in the following path: ```/tmp/my.lockfile```. Now you can name this file whatever you want, but I suggest that you name it something identifiable as to what your cron command is doing. </p><p>Another facet to keep in mind, -n tells flock to not wait for the command to finish, but to exit. (use/remove this at your own discretion). </p>