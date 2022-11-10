FROM python:3.8.15

    # python
ENV PYTHONFAULTHANDLER=1 \
    PYTHONUNBUFFERED=1 \
    # prevents python creating .pyc files
    PYTHONDONTWRITEBYTECODE=1 \
    # pip
    PIP_NO_CACHE_DIR=off \
    PIP_DISABLE_PIP_VERSION_CHECK=on \
    PIP_DEFAULT_TIMEOUT=100 \
    # poetry
    # https://python-poetry.org/docs/configuration/#using-environment-variables
    POETRY_VERSION=1.2.2 \
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
  && apt-get install -y build-essential unzip wget xxd \
  && rm -rf /var/lib/apt/lists/*

RUN curl -sSL https://install.python-poetry.org | /usr/local/bin/python3.8 -
RUN python3 -m pip install python-semantic-release

WORKDIR ${APP_PATH}
