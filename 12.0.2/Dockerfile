FROM ubuntu:24.04 AS buildexamples
RUN apt-get update \
    && apt-get install -y git \
    && rm -rf /var/lib/apt/lists/* \
    && mkdir -p /home/gurobi \
    && rm -rf /var/lib/apt/lists/*
WORKDIR /home/gurobi
RUN git init \
    && git remote add origin https://github.com/Gurobi/modeling-examples.git \
    && git fetch \
    && git checkout -t origin/master -f \
    && rm -rf .git

FROM python:3.10-slim-bookworm

ARG GRB_VERSION=12.0.2

RUN apt-get update && apt-get install -y --no-install-recommends --fix-missing \
        gcc \
        python3-dev \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*

RUN python -m pip install \
        notebook \
        matplotlib \
        numpy \
        pandas \
        scikit-learn \
        folium \
        xlrd==1.2.0 \
    && python -m pip install gurobipy==${GRB_VERSION} \
    && mkdir -p /home/gurobi
WORKDIR /home/gurobi
COPY --from=buildexamples /home/gurobi .
ENTRYPOINT ["jupyter", "notebook", "--no-browser", "--allow-root", "--ip=0.0.0.0", "--port=8888" ]
CMD ["--notebook-dir=/home/gurobi", "--NotebookApp.token=''","--NotebookApp.password=''"]
EXPOSE "8888"