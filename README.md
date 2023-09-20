## Evalquiz

This repo includes a Docker Compose config for running a complete server stack of all Evalquiz services:

- Nginx reverse-proxy service, `evalquiz-proxy`:
  - Maps frontend to `\` and to `evalquiz-client-flask`, which is a gRPC translation service to `\server\`
- Evalquiz gRPC translation layer: `evalquiz-client-flask`
  - Manages file uploads/requests to `material-server` and `pipeline-server`.
- Evalquiz material service, `evalquiz-material-server`:
  - Serves and manages materials under their respective file hashes. One file exists only once and is referenced/queried by its hash
- Evalquiz pipeline service, `evalquiz-material-server`:
  - Evalquiz core functionality of material filtering, question generation and question evaluation. This is the heart of the project
- Evalquiz material database, `evalquiz-material-db`:
  - Manages state about lecture material hashes and their location in the file system
- Evalquiz pipeline database, `evalquiz-pipeline-db`
  - Manages state about lecture material hashes and their location in the file system

### Project status

This section describes the implementation status of the project.

- Evalquiz pipeline service

  - Evalquiz config iteration
    - [x] Strongly typed interfaces and datatypes via Protobuf
    - [x] Material filtering
      - [ ] Material caching (hard to test during production of state is handled correctly)
    - [x] Question generation
    - [x] Question evaluation
      - [x] Language model evaluation
      - [ ] Other evaluation modules
    - [x] Question regeneration (implemented directly in Question generation module)
      - [x] Complete
      - [x] Overwrite (needs further testing)
      - [x] ByMetrics (needs further testing)
    - Pipeline execution
      - [x] Linear non-threaded pipeline execution
      - [x] gRPC status responses about pipeline status

- Evalquiz material service

  - [x] Material upload/download by hash
  - [x] Optional names for materials
  - [x] Queries for available names and hashes

- Evalquiz frontend
  - [x] Material upload/download with name field
  - [x] Searchable list of available materials
  - [x] Customizable config building via JSONForms
    - [x] Batches
      - [x] Capabilites
      - [x] Lecture materials
        - [x] Searchable material suggestion of materials available at material service.
      - [x] Selection of question to generate with direct answer mapping
  - [x] Raw JSON config view
  - [x] Config iteration
  - [ ] Notifications about server status during config iteration
