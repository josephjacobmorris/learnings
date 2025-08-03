# Inter Service Communication

## Different modes

**API Architectural Styles:**

### 1. **RESTful**: 
A widely adopted, stateless approach using standard HTTP methods (GET, POST, PUT, DELETE) to interact with APIs.
### 2. **SOAP**: 
An XML-based protocol that enables secure, reliable communication between systems, ideal for enterprise-grade applications and complex transactions.
### 3. **GraphQL**: 
A query language that fetches only the required data, reducing over/under-fetching and improving performance.
### 4. **gRPC**:
A high-performance protocol built on Protocol Buffers, providing efficient and real-time communication with lightweight support.
### 5. **WebSockets**:
A persistent, bidirectional communication mechanism for real-time updates, suitable for applications requiring low-latency interactions (e.g., chat services).

#### Common Use cases

##### 1. **Real-Time Chat Applications**

**Explanation:** Enables instant messaging with bi-directional communication between clients and servers.
**Advantage:** Unlike HTTP polling, WebSockets provide low-latency, continuous data flow without repeated requests.



##### 2. **Live Sports Scores**

**Explanation:** Pushes real-time score updates and statistics to users without refreshes.
**Advantage:** Offers faster updates compared to polling APIs and avoids unnecessary bandwidth usage.



##### 3. **Online Gaming**

**Explanation:** Facilitates real-time multiplayer interactions like movement, actions, and game state.
**Advantage:** Reduces latency and network overhead crucial for fast-paced gameplay, unlike HTTP.



##### 4. **Stock Market and Crypto Feeds**

**Explanation:** Delivers live price updates and market depth to trading dashboards.
**Advantage:** Continuous feed without request-response cycles allows sub-second updates essential for trading.



##### 5. **Collaborative Document Editing (e.g., Google Docs)**

**Explanation:** Syncs user edits in real-time across multiple clients.
**Advantage:** More efficient and immediate than polling or long-polling mechanisms.



##### 6. **Online Auctions**

**Explanation:** Updates bids and auction status in real-time for all participants.
**Advantage:** Real-time feedback ensures fairness and competitiveness compared to HTTP where data may lag.



##### 7. **Live Location Tracking (e.g., cab apps)**

**Explanation:** Streams the current location of vehicles to clients continuously.
**Advantage:** Eliminates repeated polling and provides accurate movement representation in real time.



##### 8. **IoT Device Monitoring**

**Explanation:** Sends telemetry data from sensors or devices to a central server continuously.
**Advantage:** Low overhead and persistent connection make it ideal for constrained devices and networks.



##### 9. **Financial Dashboards**

**Explanation:** Displays real-time metrics, KPIs, and logs for operational finance teams.
**Advantage:** Dynamic updates without refreshing the page or issuing repeated fetch calls.



##### 10. **Customer Support Live Chat**

**Explanation:** Enables agents and users to chat in real-time with presence indicators.
**Advantage:** Better UX with fast interactions, unlike refresh-heavy HTTP forms.



##### 11. **Social Media Notifications**

**Explanation:** Pushes likes, comments, messages, and alerts instantly to users.
**Advantage:** Improves engagement and immediacy compared to periodic polling.



##### 12. **Real-Time Video Streaming Metadata (not video itself)**

**Explanation:** Syncs playback status, viewer counts, or chat comments during live broadcasts.
**Advantage:** Efficient push of metadata updates in sync with video content.



##### 13. **Collaborative Whiteboards / Drawing Apps**

**Explanation:** Reflects real-time drawing strokes or object changes by other users.
**Advantage:** Enables fluid co-creation compared to lag-prone polling.



##### 14. **Real-Time Voting / Polling**

**Explanation:** Updates vote counts and visualizations as users cast votes.
**Advantage:** Enhances user participation and transparency without delays.



##### 15. **Real-Time Multiplayer Board Games**

**Explanation:** Sends game states like dice rolls or moves instantly to all players.
**Advantage:** Ensures synchronized game experience over unreliable HTTP polling.



##### 16. **Live Auction Countdown Timers**

**Explanation:** Keeps all bidders synchronized with an accurate countdown.
**Advantage:** WebSockets prevent clock desync or latency from HTTP intervals.



##### 17. **Real-Time Error/Log Monitoring (DevOps)**

**Explanation:** Streams server logs, application metrics, or alerts in live dashboards.
**Advantage:** Developers and ops get immediate visibility compared to pulling logs periodically.



##### 18. **Ride Hailing App Driver/Passenger Matching**

**Explanation:** Matches riders with nearby drivers instantly based on live events.
**Advantage:** Fast bi-directional updates lead to faster matching and reduced wait times.



##### 19. **Online Classroom Participation**

**Explanation:** Handles live hand-raise, polling, Q\&A, or quizzes during lectures.
**Advantage:** Interactive experience that’s not feasible with HTTP latency.



##### 20. **Cryptocurrency Order Book Updates**

**Explanation:** Continuously updates live order books as trades happen.
**Advantage:** High-frequency updates without request overhead improve trading decision speed.




### 6. **MQTT** (Message Queue Telemetry Transport):
A lightweight messaging protocol designed for low-bandwidth IoT devices and real-time data delivery.

## References
* https://substack.com/@sketech/note/c-79368836?r=40ln3j
* [Common Websocket Usecases](https://blog.algomaster.io/p/websocket-use-cases-system-design?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true)
* [Common Websocket Usecases Chatgpt](https://chatgpt.com/share/688ee807-4414-800c-ba31-cd5a15bdb301)