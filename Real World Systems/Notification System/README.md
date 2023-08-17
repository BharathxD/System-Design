<p><a target="_blank" href="https://app.eraser.io/workspace/vpB2BC9UzFnNrhbPQ0WT" id="edit-in-eraser-github-link"><img alt="Edit in Eraser" src="https://firebasestorage.googleapis.com/v0/b/second-petal-295822.appspot.com/o/images%2Fgithub%2FOpen%20in%20Eraser.svg?alt=media&amp;token=968381c8-a7e7-472a-8ed6-4a6626da5501"></a></p>

# Design a notification system
![Arch 1](../../Images/Notification/notification-system.png "")

This design system basically handle all kinds of notifications, from reminders to mass campaign notification

- MetaDB to store the templates
- Redis (With Bloom Filter) to keep track of notifications (If they are sent to the user or not)
- SQS queues to ensure resiliency
- Load Balancer to evenly distribute the load and ensure availability



<!--- Eraser file: https://app.eraser.io/workspace/vpB2BC9UzFnNrhbPQ0WT --->