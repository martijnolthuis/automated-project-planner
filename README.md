# Modernizing Stocktaking Planning: A Web-Based Solution

## Overview

This web application aims to modernize and simplify stocktaking planning. Traditionally, stocktaking planning has been a time-consuming and complex task. With this tool, users can intuitively upload, adjust, and plan store data, all through a user-friendly interface.

## Table of Contents

- [Features](#features)
- [Technology Stack](#technology-stack)
- [Installation and Setup](#installation-and-setup)
- [Usage](#usage)
- [License](#license)

## Features

- **Easy Data Upload**: Users can quickly and effortlessly upload Excel files containing store data.
- **Interactive Planning Interface**: Drag-and-drop interactions facilitate effortless adjustments to store schedules. Metrics provide real-time insights into the impact of planning decisions.
- **Smart Allocation**: Utilize the "Assign Stores to Days" algorithm for intelligent distribution of stores across weekdays, ensuring balanced employee allocation.

## Technology Stack

- **Front-end**: Vue.js
- **Utilities**: Vue CLI, Docker, and Docker-compose

## Installation and Setup

1. Clone this repository:
   `git clone https://github.com/martijnolthuis/automated-project-planner`
2. Navigate to the directory:
   `cd automated-project-planner`
3. Use Docker-compose to build and run the application:
   `docker-compose up --build`
4. Access the application in your browser at `http://localhost:8080`.

## Usage

1. Begin by uploading your Excel file containing store data through the front-end interface.
2. Use the drag-and-drop feature to manually assign stores or deploy the algorithm for automated assignments.
3. Monitor metrics and make necessary adjustments for optimal planning.

## License

This project is licensed under the MIT License. View the `LICENSE` file in the repository for details.
