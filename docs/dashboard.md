# Actuated Dashboard

The actuated dashboard is available to customers for their enrolled organisations.

For each organisation, you can see:

* Today's builds so far - a quick picture of today's activity across all enrolled organisations
* Runners - Your build servers and their status
* Build queue - All builds queued for processing and their status
* CLI - install the CLI for management via command line
* Insights - full build history and usage by organisation, repo and user

## Today's builds so far

On this page, you'll get today's total builds, total build minutes and a break-down on the statuses - to see if you have a majority of successful or unsuccessful builds.

![Today at a glance](/images/today-glance.png)
> Today's activity at a glance

Underneath this section, there are a number of tips for enabling a container cache, adjusting your subscription plan and for joining the actuated Slack.

More detailed reports are available on the insights page.

## Runners

Here you'll see if any of your servers are offline, in a draining status due a restart/update or online and ready to process builds.

![Runner view](/images/dashboard/runners.png)

The Ping time is how long it takes for the control-plane to check the agent's capacity.

## Build Queue

Only builds that are queued (not yet in progress), or already in progress will be shown on this page.

![Build queue](/images/dashboard/build-queue.png)

Find out how many builds are pending or running across your organisation and on which servers.

## Insights

Three sets of insights are offered - all at the organisation level, so every repository is taken into account.

You can also switch the time window between 28 days, 14 days, 7 days or today.

The data is contrasted to the previous period to help you identify spikes and potential issues.

The data for reports always starts from the last complete day of data, so the last 7 days will start from the previous day.

### Build history and usage by organisation.

Understand when your builds are running at a higher level - across all of your organisations - in one place.

![Total organisation usage](/images/dashboard/org-usage.png)

You can click on *Minutes* to switch to total time instead of total builds, to see if the demand on your servers is increasing or decreasing over time.

### Build history by repository

![Repository based usage history](/images/dashboard/repo-usage.png)

When viewing usage at a repository-level, you can easily identify anomalies and hot spots - like mounting build times, high failure rates or lots of cancelled jobs - implying a potential faulty interaction or trigger.

### Build history per user

![Build history per user of your GitHub organisation](/images/dashboard/user-usage.png)

This is where you get to learn who is trigger the most amount of builds, who may be a little less active for this period and where folks may benefit from additional training due a high failure rate of builds.
