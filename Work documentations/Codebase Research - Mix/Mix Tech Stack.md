- Notion sometimes failed to fetch -> just run build again and again.
### Data flow

- User request to mix.day 
	- -> DNS resolves to load balancer 
		- -> [[Load Balancer]] picks the "healthy" server
			- -> Next.js renders the page & make GraphQL queries
				- -> Hasura translates GraphQL -> SQL
					- -> PostgreSQL returns data

### Hasura

Hasura automatically reads your PostgreSQL database structure and instantly creates a complete GraphQL API with real-time subscriptions -> never have to write backend API code.

### k8s

Kubernetes automatically manages and scales your containerized applications across multiple servers, ensuring they stay running, healthy, and properly distributed without manual intervention.

### **ECS Fargate** - Running the Application

- **What it does**: Runs your application code without managing servers
- **Think of it as**: A magic box where you put your app, and AWS handles all the computer hardware
- **For Mix**: Runs the Next.js website and Hasura API
- **Benefit**: Automatically scales up when busy, scales down when quiet

### **Application Load Balancer (ALB)** - Traffic Director

- **What it does**: Distributes incoming website visitors across multiple servers
- **Think of it as**: A traffic cop directing cars to different lanes to prevent jams
- **For Mix**: Makes sure mix.day doesn't crash when lots of people visit at once
- **Benefit**: If one server fails, visitors get redirected to working servers

### **Route 53** - Domain Name System

- **What it does**: Translates "mix.day" into the actual server address
- **Think of it as**: Like a phone book that converts "John's Pizza" to an actual phone number
- **For Mix**: When you type mix.day, Route 53 finds the right server
- **Benefit**: Fast, reliable domain management

### **CloudWatch** - Monitoring & Logging

- **What it does**: Records what happens in your application (errors, performance, etc.)
- **Think of it as**: Security cameras + performance dashboard for your app
- **For Mix**: Developers can see if something breaks and why
- **Benefit**: Quick problem detection and debugging

### **S3** - File Storage

- **What it does**: Stores files (images, documents, static websites)
- **Think of it as**: A massive, reliable hard drive in the cloud
- **For Mix**: Hosts their Storybook component library
- **Benefit**: Cheap, virtually unlimited storage

### **Copilot** - Deployment Tool

- **What it does**: Automates setting up and managing all the above AWS services
- **Think of it as**: A recipe that automatically sets up your entire kitchen
- **For Mix**: Developers can deploy with simple commands instead of manual setup
- **Benefit**: Consistent, repeatable deployments