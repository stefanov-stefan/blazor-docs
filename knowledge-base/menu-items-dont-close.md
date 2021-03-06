---
title: Menu items do not close when hovered over
description: Menu items do not close when hovered over
type: troubleshooting
page_title: Menu items do not close when hovered over
slug: kb-menu-items-dont-close
position: 
tags: 
ticketid: 1479006
res_type: kb
---

## Description

I have setup a [Menu]({%slug components/menu/overview%}) in my Telerik Blazor Application. When the component is hosted and the user hovers quickly over the menu items they would not always close.

>caption The menu items do not close when the user hovers quickly over them

![menu items do not close on hover](images/menu-items-dont-close.gif)
   
## Cause\Possible Cause(s)

This behavior is mainly observed when the application uses the `Blazor Server` hosting model. It occurs mostly due to high network latency to the server which hosts the project because every user interaction involves a network hop (passing one data package from one network segment to the next). 

The SignalR connection is asynchronous and the large volume of quick events are not guaranteed to arrive in the same order as they are sent, which can cause issues with the circuit state - the server will receive wrongly ordered packets and events and can render wrong popups, thinking previous ones are not open or are already closed.

A common reason for such a problem is that WebSockets are not enabled on the hosting server (most common in cloud hosting such as Azure, where they are turned off by default).

Another common cause is the usage of a VPN to access the server - such tools often add a lot of latency to the network calls, and that is very detrimental to the server-side Blazor flavor.

## Solution

A solution to this would be using the `Blazor WebAssembly` hosting model. That would remove the latency factor since the content of the application is executed on the device of the user.

Any other option that lowers the user latency to reasonable values for the server-side Blazor model can also resolve the problem.