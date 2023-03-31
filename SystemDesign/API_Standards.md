# API Standards

## Rules to follow when creating API for effective and safety
![](/home/joseph/Pictures/Screenshot from 2023-03-31 10-35-27.png)
### Notes
* Use resource names (avoid verbs use nouns)
* Use plurals for resource names
* Use API versioning to support backward compatibility & breaking changes handling
* Query (do GET) after soft deletion (practice of having a field in database to track whether row has been deleted instead of actually deleting) 
* Pagination helps reduce the size of the payload and helps in reducing latency
* Add rate limit based on IP address, user, action group (group that user belongs)

## References
* https://lnkd.in/g9wAgcke