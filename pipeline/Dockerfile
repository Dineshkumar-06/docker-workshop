# base Docker image that we will build on
FROM python:3.13.11-slim

# Copy uv binary from official uv image (multi-stage build pattern)
COPY --from=ghcr.io/astral-sh/uv:latest /uv /bin/

# set up the working directory inside the container
WORKDIR /code

# Add virtual environment to PATH so we can use installed packages, this helps us run the pipeline which adding "uv" , "run" in the entry point since the python directly runs from the this path
ENV PATH="/code/.venv/bin:$PATH"

# copy dependencies for uv containing the details of dependency packages
COPY pyproject.toml .python-version uv.lock ./

# Install dependencies from lock file (ensures reproducible builds)
RUN uv sync --locked

# # copy the script to the container. 1st name is source file, 2nd is destination
COPY pipeline.py .

# define what to do first when the container runs
# in this example, we will just run the script
ENTRYPOINT ["python", "pipeline.py"]