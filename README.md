# DataDigger Database Schema

This repository contains the finalized **PostgreSQL database schema** for the DataDigger client application. It includes the Entity-Relationship Diagram (ERD), DBML schema, and visual documentation to support database understanding, collaboration, and deployment.

## Repository Structure

**Tables/** # Contains schema definitions for individual tables
**Chart Economy.png** # Visual chart showing economy-related relationships
**DataDigger.png** # General database architecture overview
**ERDDatabaseChartEconomy.png** # Complete ERD for the database
**database.dbml** # Source schema in DBML format (for tools like dbdiagram.io)


## Tech Stack

- **Database**: PostgreSQL  
- **Schema Format**: DBML  
- **Documentation**: PNG diagrams & table breakdowns

## Purpose

This database was designed to power a client-facing analytics platform for DataDigger. It ensures:
- Efficient data normalization
- Scalable structure for future extensions
- Clear relationships between business-critical entities

# Usage

1. Open the `.dbml` file to visualize or edit the schema.
2. Use the `.png` files to reference entity relationships and architecture during development.
3. PostgreSQL migrations or table creation scripts can be generated from the DBML schema.

## Contribution

This schema is currently managed internally. For suggestions or improvements, please open an issue or submit a pull request.

## License

This project is private and intended for internal use. Licensing terms will be updated upon project deployment.



