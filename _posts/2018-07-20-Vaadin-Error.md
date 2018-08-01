---
layout: post
title:  Failed to load the widgetset (after clean installation)
date: 2018-07-18
categories: Vaadin
tags: [Vaadin]
description: Failed to load the widgetset
published: true
---

    com.vaadin.server.VaadinServlet serveStaticResourcesInVAADIN
    INFO: Requested resource [/VAADIN/widgetsets/com.mypackage.client.ui.AppWidgetSe
    t/com.mypackage.client.ui.AppWidgetSet.nocache.js] not found from filesystem or
    through class loader. Add widgetset and/or theme JAR to your classpath or add fi
    les to WebContent/VAADIN folder.

mvn vaadin:update-widgetset install

https://vaadin.com/forum/thread/2541089

