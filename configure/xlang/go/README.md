# Go language server

This folder contains the deployment files for the Go language server.

You can enable it by:

1. Append the `kubectl apply` command for the Go language server deployment to `kubectl-apply-all.sh`.

   ```bash
   echo kubectl apply --prune -l deploy=xlang-go -f configure/xlang/go/ --recursive >> kubectl-apply-all.sh
   ```

1. Add the following environment variables to the `lsp-proxy` deployment to make it aware of the Go language server's existence.

   ```yaml
   # base/lsp-proxy/lsp-proxy.Deployment.yaml
   env:
     - name: LANGSERVER_GO_BG
       value: tcp://xlang-go-bg:4389
   ```

1. Add the following entry for the Go language server to the `langservers` array in your site configuration.

   ```yaml
   # base/config-file.ConfigMap.yaml

   config.json: |-
     {
       "langservers": [
         {
           "language": "go",
           "address": "tcp://xlang-go:4389"
         }
       ]
     }
   ```

1. Apply your changes to `lsp-proxy`, `base/config-file.ConfigMap.yaml`, and the Go language server to the cluster.

   ```bash
   ./kubectl-apply-all.sh
   ```
