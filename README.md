# aws 

This task performs operations on Amazon Web Services resources using `aws`.

## Install the Task

```
kubectl apply -f https://raw.githubusercontent.com/tektoncd/catalog/v1beta1/aws-cli/aws-cli.yaml
```

## Parameters

- **SCRIPT**: The script to run on `aws` CLI e.g `aws s3 ls` ( default: `aws $@` ).
- **ARGS**: The arguments to pass to `aws` CLI, which are appended 
    to `aws` e.g. `s3 ls` ( default: `["help"]` ).


## Workspace

- **source**: To mount file which is to be uploaded to the aws resources, 
    this should be mounted using `ConfigMap`.


## Secret

AWS `credentials` and `config` both should be provided in the form of `secret`,
 which is mounted as `volume` in the task.

[This](./example/secret.yaml) example can be referred to create `aws-secrets`.


## Usage

After creating the task, you should now be able to execute `aws` commands by 
specifying the command you would like to run as the `ARGS` or `SCRIPT` param. 

The `ARGS` param takes an array of aws subcommands that will be executed as 
part of this task and the `SCRIPT` param takes the command that you would like to run on aws CLI.

Following `command` can be used to create `ConfigMap` from the `file`.
```
kubectl create configmap upload-file --from-file=demo.zip
```
Here `upload-file` is the name of the `ConfigMap`, this can be changed based on the requirements.

See [here](./example/run.yaml) for example of `aws s3` command.


### Note


- Either `SCRIPT` or `ARGS` must be provided in the `params` of the task.

- In case there is no requirement of file that is to be uploaded on the aws resources,
 then `emptyDir` needs to be mounted in the `workspace` as shown below:
    ```
    workspaces:
      - name: images-url
        emptyDir: {}
    ```
    otherwise, if ConfigMap is needed:

    ```
    workspaces:
      - name: images-url
        configmap:
            name: upload-file 
    ```