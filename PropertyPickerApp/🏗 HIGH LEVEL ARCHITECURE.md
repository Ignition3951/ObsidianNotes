

1. 🌐 Client Layer

- **Web App (React/Angular/Vue)**
- **Mobile App (Flutter/React Native)**
- **Public CDN**: Serves static assets (HTML, CSS, JS, images)

2. 🌍 Edge Layer

- **Reverse Proxy (NGINX)**

	- SSL termination
	- Load balancing
	- Rate limiting

- **API Gateway (Spring Cloud Gateway)**

	- Request routing
	- Authentication (JWT/OAuth2)
	- Throttling & logging

3. 🔧 Microservices Layer (Dockerized, K8s Orchestrated)

[[PropertyPickerHLD]]

All services are:

- Containerized with **Docker**
- Managed via **Kubernetes**
- Communicate via **REST**
- Use **Spring Boot + Spring Security**

4. 🗃 Data Layer

	1. User logs in → JWT token issued
	2. Token sent with each request → Spring Security validates
    3. Role-based access filters endpoints
    4. Users interact with property listings based on role

5. 📬 Messaging & Eventing

- **Kafka / RabbitMQ**

- Event-driven architecture
- Used for notifications, analytics, async processing

6. 🔐 Security

- **Spring Security**: Role-based access control
- **JWT**: Stateless authentication
- **OAuth2.0**: Social login (Google, Facebook)
- **API Gateway**: Centralized auth enforcement

7. 📊 Observability

- **Prometheus + Grafana**: Metrics and dashboards
- **ELK Stack**: Centralized logging
- **Jaeger**: Distributed tracing