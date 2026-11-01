FROM python:3.12.15

# pinned tool binaries (tracked by Dependabot via COPY --from)
COPY --from=ghcr.io/astral-sh/uv:0.13.6 /uv /uvx /usr/local/bin/
COPY --from=ghcr.io/astral-sh/ruff:0.17.3 /ruff /usr/local/bin/
COPY --from=ghcr.io/astral-sh/ty:0.0.95 /ty /usr/local/bin/
COPY --from=ghcr.io/cocogitto/cog:7.0.0 /usr/bin/cog /usr/local/bin/
COPY --from=ghcr.io/jqlang/jq:1.7.1 /jq /usr/local/bin/
COPY --from=ghcr.io/mikefarah/yq:4.54.1 /usr/bin/yq /usr/local/bin/

    # python
ENV PYTHONFAULTHANDLER=1 \
    PYTHONUNBUFFERED=1 \
    # prevents python creating .pyc files
    PYTHONDONTWRITEBYTECODE=1 \
    # pip
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    # uv: avoid hardlink warnings when cache and project are on different filesystems
    UV_LINK_MODE=copy \
    # poetry
    # https://python-poetry.org/docs/configuration/#using-environment-variables
    POETRY_VERSION=2.5.1 \
    # make poetry install to this location
    POETRY_HOME="/opt/poetry" \
    # make poetry create the virtual environment in the project's root
    # it gets named `.venv`
    POETRY_VIRTUALENVS_IN_PROJECT=true \
    # do not ask any interactive question
    POETRY_NO_INTERACTION=1 \
    POETRY_CACHE_DIR="/opt/poetry/.cache" \
    # POETRY_CACHE_DIR is not respected currently see https://github.com/python-poetry/poetry/issues/2445#issuecomment-670829271
    XDG_CACHE_HOME="/opt/poetry/.cache" \
    # paths
    VENV_PATH="/opt/venv" \
    APP_PATH="/app"

# prepend poetry and venv to path
ENV PATH="/usr/local/bin/:$POETRY_HOME/bin:$VENV_PATH/bin:$PATH"

RUN apt-get update \
  && apt-get install -y build-essential unzip wget xxd vim-common\
  && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://install.python-poetry.org | /usr/local/bin/python3.12 - 
RUN python3 -m pip install python-semantic-release

WORKDIR ${APP_PATH}
