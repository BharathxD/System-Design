# Design a notification system

![Arch 1](../../Images/Notification/notification-system.png)

This design system basically handle all kinds of notifications, from reminders to mass campaign notifcation

- MetaDB to store the templates
- Redis (With Bloom Filter) to keep track of notfications (If they are sent to the user or not)
- SQS queues to ensure resiliency
- Load Balancer to evenly distribute the load and ensure availability
