FROM vllm/vllm-openai:v0.10.0

# `vllm[audio]` is an optional dependency that is not included in vllm's docker image due to licensing issues.
# See <https://github.com/vllm-project/vllm/issues/13675#issuecomment-2676110072>
# NOTE: transformers 4.53 introduced a bug that causes the server to crash.
# See <https://github.com/huggingface/transformers/pull/39244>
# The 4.54.0 release includes the fix.
RUN pip install vllm[audio] transformers>=4.54.0
