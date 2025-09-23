# Homework from Lecture 02 (not mandatory)

Teams should practice the concepts learned in Lecture 02 by implementing a simple full-stack application.

## Team Practice Exercises

### 1. Project Setup
- [ ] **Create new React + ASP.NET project** - Initialize a React + ASP.NET Core application project from template
- [ ] **Test basic connection** - Verify React can communicate with ASP.NET Core

### 2. Backend Development (ASP.NET Core)
- [ ] **Create a simple model** - Define a data model (e.g., Product, User, Task)
- [ ] **Implement a controller** - Create API controller with basic endpoints:
  - GET /api/[resource] - Get all items
  - POST /api/[resource] - Create new item
  - PUT /api/[resource]/{id} - Update existing item
  - DELETE /api/[resource]/{id} - Delete item
- [ ] **Add sample data** - Create in-memory data or mock service
- [ ] **Test endpoints** - Verify all API endpoints work correctly

### 3. Frontend Development (React)
- [ ] **Create components** for displaying data
- [ ] **Implement API calls** - Use fetch() or axios to communicate with backend
- [ ] **Handle loading states** - Show loading indicators during API calls
- [ ] **Display data** - Render API responses in React components
- [ ] **Add error handling** - Handle and display API errors gracefully

### 4. API Testing
- [ ] **Install Postman** (if not already installed)
- [ ] **Create API collection** - Organize requests for your endpoints
- [ ] **Test all HTTP methods** - GET, POST, PUT, DELETE
- [ ] **Verify responses** - Check status codes and response data
- [ ] **Test error scenarios** - Try invalid requests and verify error handling

### 5. Integration & Polish
- [ ] **Connect frontend to backend** - Full end-to-end functionality
- [ ] **Style the application** - Basic CSS or component library
- [ ] **Add form validation** - Client-side and server-side validation
- [ ] **Test user workflows** - Complete create, read, update, delete operations

## Deliverables

### Required
- [ ] **Working full-stack application** with React frontend and ASP.NET Core backend
- [ ] **At least 3 API endpoints** demonstrating different HTTP methods
- [ ] **React components** that consume and display API data
- [ ] **Postman collection** with all your API requests

## Resources

- [PSI2025-Playground Lecture 02](https://github.com/niku-live/PSI2025-Playground/tree/lectures/02)
- [React Documentation](https://react.dev/)
- [ASP.NET Core Documentation](https://docs.microsoft.com/en-us/aspnet/core/)
- [RESTful API Design Best Practices](https://docs.microsoft.com/en-us/azure/architecture/best-practices/api-design)
- [Postman Documentation](https://learning.postman.com/docs/getting-started/introduction/)