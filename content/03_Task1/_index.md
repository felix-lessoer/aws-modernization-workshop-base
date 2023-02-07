---
title: "Find issues in applications running on AWS"
weight: 40
---
## Task 1

In the first task you have to find the root cause of an issue within our sample application. You can see that there is an issue because of the existence of error messages.

You can find error messages within the logs and also by looking at the APM data. Both ways will lead to the expected results. Choose the one that is more preferably for you. 

You know that you’ve fixed the issue when there is no new error message coming in.
![Elastic environment](/images/task1-solution.png)

## After Task 1

You now have seen how a dashboard could be used to dive into the data and perform a root cause analysis for some simple issues. Elastic offers different kind of visualizations. Another very handy way to visualize your data is using Canvas. In contrast to the dashboard you have used to solve task 1, the Canvas dashboard is more graphical and less interactive. However it also has some unique capabilties like attaching metric data next to your e.g. architecture diagrams. That can help a lot in understanding where the issue is coming from.
We've prepared a Canvas Dashboard in your Environment that visualizes the different ways log data is flowing into your cluster. Find the Canvas board in your environment and double check that all the data flows are working fine.  

