# Repository Guidelines

## Project Structure & Module Organization
HelenusDB is a multi-module Maven project (parent `pom.xml`) targeting Java 17. Primary modules:

- `core/`: shared abstractions (`Entity`, `Identifier`, exceptions).
- `document/`: document-oriented Cassandra repository and schema helpers.
- `index/`: in-memory indexing implementations (trie, suffix, B+ tree, inverted index).
- `transact/`: unit-of-work and change tracking primitives.
- `evolve/`: schema evolution utilities for Cassandra.

Code follows Maven layout: `src/main/java` for production code and `src/test/java` for tests. Package namespace is primarily `com.helenusdb.*`.

## Build, Test, and Development Commands
- `mvn clean verify`: full multi-module build + tests.
- `mvn test`: run all tests without packaging.
- `mvn -pl index test`: run tests for one module only.
- `mvn -pl document -am package`: build a module and required dependencies.
- `mvn -DskipTests package`: compile/package quickly when tests are not needed.

Run commands from repository root unless you are intentionally working inside a single module.

## Coding Style & Naming Conventions
- Use Java 17 language features conservatively; keep public APIs stable.
- Follow existing style in each file (tabs/spaces, brace placement); do not reformat unrelated code.
- Class names: `PascalCase`; methods/fields: `camelCase`; constants: `UPPER_SNAKE_CASE`.
- Keep packages under `com.helenusdb.<module>...`.
- Prefer clear, small classes and explicit exceptions over hidden runtime behavior.

## Testing Guidelines
- Test framework: JUnit 5 (`junit-jupiter`).
- Test classes use `*Test.java`; performance-oriented tests may use `*BenchmarkTest.java`.
- Keep tests near the module they validate (`<module>/src/test/java`).
- For focused runs, use `mvn -pl <module> -Dtest=ClassName test`.
- Add or update tests for any behavior change, especially key parsing, indexing behavior, and unit-of-work flows.

## Commit & Pull Request Guidelines
- Commit messages in this repo are short, imperative/past-tense summaries (for example: `Added UTF-8 encoding`, `Updated submodules`).
- Keep subject lines concise and scoped to one logical change.
- PRs should include: purpose, affected modules, test evidence (`mvn test` output summary), and linked issue(s) when applicable.
- Include usage examples or API notes when changing public behavior.

## Security & Configuration Tips
- Cassandra drivers are typically `provided`; verify runtime dependencies in consumer apps.
- Do not commit credentials, local cluster configs, or generated artifacts from `target/`.
