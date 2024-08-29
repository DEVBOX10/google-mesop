# AI package

This contains all the AI-related code for Mesop. `ai/` is structured as an independent package and should have no dependencies on other parts of the codebase, nor dependents from other parts of the codebase.

All the commands should be run from the `ai/` directory.

**Directory structure:**

- All entry-points are in `src/*.py` - this includes the AI service and scripts.
- `src/common` contains code that's shared between offline scripts and the online service.

## Scripts

These are scripts used to generate and process data for offline evaluation.

### Generate run

```sh
uv run src/generate_run.py --model gpt-4o-mini --run_name simple_3 --num_inputs 2
```

> Note: look at `llm_lib.py` to see which model names you can pass in.

> Note: `num_inputs` is optional.

### Evaluate run

**Prerequisites**:

- You must run the [sandbox](#sandbox).

To evaluate the generated outputs from the previous step:

```sh
uv run src/evaluate_run.py --model gpt-4o-mini --run_name simple_3
```

This will do the following checks:

- Ensure the diff is patchable and generate the patched Python file
- Execute the patched Python file from top to bottom (note: this uses a regular Python interpreter and not the mesop CLI)
- Type-check the patched Python file

Use the [viewer](#viewer) to view the evaluated run.

### Process goldens

```sh
uv run src/process_goldens.py
```

Use the [viewer](#viewer) to view the goldens.

### Viewer

**Prerequisites**:

- You must run the [sandbox](#sandbox) in order to run the Mesop appss.

To run the viewer:

```sh
mesop viewer.py
```

- To view a previous eval run, go to: localhost:32123/?model=$model&run=$run
- To view the goldens, go to: localhost:32123/?golden=true

### Format goldens

This is used to format the golden dataset for the fine-tuning process.

```sh
uv run src/format_goldens.py
```

## Service

The service is a Flask app that provides a REST API for the AI functionality.
To run the service, run the following command:

```sh
uv run src/service.py
```

## Sandbox

Start the sandbox:

```sh
cd sandbox
docker stop mesop-sandbox;
docker rm mesop-sandbox;
docker build -t mesop-sandbox . && docker run --name mesop-sandbox -d -p 8080:8080 mesop-sandbox;
```

## Common

The common package contains the code that is shared between multiple Python binaries and scripts.

## Docbot

Docbot is a standalone app that creates a RAG-chatbot for the Mesop docs.