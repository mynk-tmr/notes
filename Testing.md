- **Unit tests** : test small isolated parts of codebase. Interaction with other units is *mocked* eg. class, methods, etc.
- **Integration tests**: test interaction across *real* unit-tested portions of code like modules, database, etc.
- **Smoke test / sanity test**: done before deep tests. Checks critical software functionality using unit+integration tests
- **Functional tests**: test business requirements of app and verify output.
- **End to End tests**: replicates a user behavior with the software.
- **Performance tests**: evaluate system's performance under different conditions, identify memory leaks, bottlenecks, and scalability issues. Major ways -> *load testing* (to identify breakpoint) and *stress testing* (to identify behaviour) under heavy load.
- **Regression tests**: ensure new changes don't break exisiting functionalities.
- **Acceptance tests**: testing against predefined acceptance criteria set by stakeholders.
- **Usability tests**: ensure software is user-friendly and improve UX
- **Fuzz tests**: test with random or unexpected inputs to identify how it responds to unexpected data.
- **Compatibility Testing**: ensuring that software works seamlessly on different platforms.

### Levels of tests
- **Whitebox tests**: test logic of app
- **Blackbox tests**: test behaviour of app, not logic
- **Greybox tests**: combine both
- **Alpha tests:** test in controlled environment
- **Beta/UAT tests:** test in real environment by selected end users