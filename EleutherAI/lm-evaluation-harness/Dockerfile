FROM python:3.13-slim AS fetcher

WORKDIR /build
RUN apt-get update \
  && apt-get install -y --no-install-recommends \
  git \
  && git clone --depth=1 https://github.com/EleutherAI/lm-evaluation-harness.git \
  && rm -rf lm-evaluation-harness/.git

FROM python:3.13-slim

COPY --from=fetcher /build/lm-evaluation-harness ./lm-evaluation-harness

RUN cd lm-evaluation-harness \
  && python -m pip install .['api'] .['ifeval'] .['longbench'] .['math'] .['multilingual']

ENTRYPOINT ["lm-eval"]

