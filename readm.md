kubectl create secret generic hf-secret --from-literal=hf_api_token=hugging-face-secret --dry-run=client -o yaml | kubectl apply -f -


kubectl apply -f vllm-2-27b-it.yaml

kubectl wait --for=condition=Available --timeout=700s deployment/vllm-gemma-deployment

kubectl logs -f -l app=gemma-server

kubectl port-forward service/llm-service 8000:8000


USER_PROMPT="I'm new to coding. If you could only recommend one programming language to start with, what would it be and why?"

curl -X POST http://localhost:8000/generate \
  -H "Content-Type: application/json" \
  -d @- <<EOF
{
    "prompt": "<start_of_turn>user\n${USER_PROMPT}<end_of_turn>\n",
    "temperature": 0.90,
    "top_p": 1.0,
    "max_tokens": 128
}
EOF

----------------------

Gradio setup

kubectl apply -f gradio.yaml

kubectl wait --for=condition=Available --timeout=300s deployment/gradio

kubectl port-forward service/gradio 8080:8080

