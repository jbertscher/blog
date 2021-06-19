---
layout: post
title:  "Questions to Help Build Better Dashboards"
date:   2021-06-19
categories: work hiring management
---
In my experience, a lot of time is wasted building dashboards. They pile up on the server with only a handful getting much use. And even those that are used aren’t fully exploited. It may be that my experience is atypical but I suspect that it will resonate with others. If you’re one of them, here are my thoughts on how you can dramatically increase the chances of your dashboard projects succeeding. 

What do dashboards that don’t get much love have in common? Assuming your stakeholders care about data, if they’re not using your dashboards it’s probably because they either don’t find them useful or they perceive using them taking too much time and energy to be worth it. There could be a few reasons for this and I may devote future articles to them but one of the most important root causes is failing to define requirements early on.

I know of a data scientist at a large tech firm who requires colleagues to complete an online form when they ask him to perform an analysis. He does this to make sure that the requester has thought enough about the problem, and that the request makes sense, before he will consider doing it. While you don’t need to resort to a Google Form, I think this is the right sort of idea.

In this article, I’m going to outline some questions I’ve learned to ask at the beginning of the dashboard-design process (and continue to ask as I iterate). These have helped me build better dashboards, have better relationships with my stakeholders, and be more efficient in the process. While every project may be different, here are some fundamental questions you may want to start with.

# Questions to ask in the dashboard-design process

## Who are your stakeholders?

Your stakeholders are the people who you need to make happy in order for your project to be a success. Being clear on this helps to avoid an often-overlooked trap: designing the report only for the person asking for it. Those using the report should be considered stakeholders as well. For instance, a customer service (CS) manager may ask for a report but intend for it to be used by CS agents.

Beware of these situations because there is additional risk that what the requestor thinks will be useful and what the end users think will be useful are not aligned. I’ve personally fallen into this trap. Try to gather requirements from the people who will actually be using your report.

## Why do they want this report and how will they use it?

Get stakeholders to explicitly outline the purpose of the dashboard in detail. Is it to make specific decisions? If that’s the case, what decisions? What would the dashboard need to say in order for a stakeholder to take a particular action? What action would they take and how? If it’s for monitoring, what would be considered something that would need to get flagged? What would be the process for discovering and flagging an anomaly?

There are two main reasons for asking these questions. First, it helps to understand whether a dashboard is the correct tool at all. Sometimes what’s really appropriate is a specific application or an ad-hoc analysis. Second, if a dashboard is appropriate, these questions put into focus what sort of information should be there and how it should be communicated in order to be effective.

## What things would they like to see? Link it to the report’s why and how.

Find out what information and types of visualisations your stakeholders have in mind for the dashboard. Make sure there’s a direct link between the dashboard’s purpose, how it will be used, and what you stakeholders expect to see. If your stakeholders can express a coherent story here, it can be extremely useful as it gives you a blueprint for what to include.

On the other hand, you may find that they have a clear idea of what they want to see but on closer inspection you discover that they won’t serve the dashboards how and what. In this case, you can manage your stakeholder’s expectations by agreeing to exclude those things.

For example, your stakeholder might say they want to see the last day’s change in sales compared to the day before because it will help them determine whether there is a problem. If your sales have high weekly seasonality then by definition you’ll have a drop in sales on some days compared to the day before but this won’t indicate a problem. In this case, you need some way of communicating a drop that’s outside of what you would expect, given the seasonality. 

## What’s the level of granularity? 

This is a basic question but one that can save time later, since it may change how you structure your data. Is this going to be broken down to a daily level or can you keep it weekly? What about a rolling 7 day period? 

## When should it be updated and at what frequency?

Even if your dashboard answers all of your stakeholders questions, it can be rendered useful by not having timely data. You may need to design your queries and calculation more efficiently if it needs to update every hour compared to once a week. It also helps when deciding where to get your data from. If the report should be ready by 8am but the tables that you’re dependent on will only finish at 8:30am then you need to think about using a different data source or manage your stakeholder’s expectations on this.

# The design process and refining your requirements

Just as important as asking the questions above is to collaborate with your stakeholders on refining them through a process of dashboard design, gathering feedback, and iterating.
Importantly, get this all down on paper in a doc that’s shared with your stakeholder and perhaps also with your manager so that it’s super transparent and everyone knows what the scope of the project is. 

There is of course much more to ensuring that your dashboards are useful, like ensuring that your stakeholders know how to use them and that your underlying data is accurate enough. Asking the above questions is a great start, especially if paired with an iterative approach, getting feedback on each stage, and refining requirements along the way.
