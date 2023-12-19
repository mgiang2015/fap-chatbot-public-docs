
# Principles and Design

This document highlights the principles and design implemented in the application.

## Software Development Principles

### Separation of Concerns

Our application consists of the frontend, backend and database applications that run separately.
In the frontend, UI-related logic, state and data management logic, and API calls logic are kept separate and in their respective folders.
In the backend, logic for handling API calls, connecting to database and connecting to the LLMs are also kept separate.

### Single Responsibility Principle

Break frontend UI into multiple smaller components, break data management logic into smaller parts depending on their usage in the components so that each component only has one reason to change

### DRY (Don't Repeat Yourself)

Design components to be reusable by encapsulating specific functionality and logic within a component, while accepting props to customise its behavior to be reused. In the backend application, common functionality such as connecting to database and sending emails is extracted into a helper function to avoid duplicating code.

## Design Patterns

### Facade

Facade can be recognized in a class that has a simple interface, but delegates most of the work to other classes. Usually, facades manage the full life cycle of objects they use. [API/app/main.py](/API/app/main.py)

### Singleton

Singleton is a creation design pattern, which ensures that only one object of its kind exists and provides a single point of access to it for any other code. In our code we use it in backend to access proposals and case studies.

### Provider

Provider is a design pattern used in react-redux, to share global data and share them down the component tree using useSelector and useDispatch hooks. Some of these states and data can be saved and reloaded to preserve data.

By using the Provider design pattern, prop drilling can be avoided, where data has to be passed to child components deep in the component tree, especially when the application expands and more nested components are introduced and we have to explicitly pass props through each level of the component tree.

However, a mix of reducers and props passing is used to balance between the centralized state management and component-specific data sharing, to promote modularity and reusability in our application.