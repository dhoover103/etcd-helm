# etcd-helm
Sample etcd operator using environment variables for the image location


## Update your Helm operator

Make sure your Helm chart only references the values file for images. Each image should be a single value (it can't be split into repository and tag, for example).

      containers:
      - name: {{ template "etcd.fullname" . }}
        image: "{{ .Values.image.image }}"
        imagePullPolicy: "{{ .Values.image.pullPolicy }}"

In the watches.yaml file, add a field for overrideValues. It should contain each image value, should be set to an environment variable

  overrideValues:
    image.image: $RELATED_IMAGE_STATEFULSET

NOTE: the variable name MUST follow the pattern RELATED_IMAGE_<identifier>. There is code looking for that string in your operator.

Using operator-sdk version 0.14.0 or later, build the updated operator image.

Define the environment variables in the operator container. In the CSV and operator.yaml, declare the variable and set it to a default value.

          env:
            - name: RELATED_IMAGE_STATEFULSET
              value: k8s.gcr.io/etcd-amd64:3.2.26

