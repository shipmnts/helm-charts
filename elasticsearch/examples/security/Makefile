default: test

include ../../../helpers/examples.mk

RELEASE := elastic

install:
	helm upgrade --install --values ./security.yml $(RELEASE) ../../ --set replicas=1 ; \
	
purge:
	helm del --purge $(RELEASE)

test: secrets install goss

secrets:
	kubectl delete secrets elastic-credentials elastic-certificates elastic-certificate-pem || true && \
	vault read -field=value secret/devops-ci/helm-charts/elasticsearch/security/certificates | base64 --decode > elastic-certificates.p12 && \
	vault read -field=value secret/devops-ci/helm-charts/elasticsearch/security/certificate-pem | base64 --decode > elastic-certificate.pem && \
	kubectl create secret generic elastic-credentials  --from-literal=password=changeme --from-literal=username=elastic && \
	kubectl create secret generic elastic-certificates --from-file=elastic-certificates.p12 && \
	kubectl create secret generic elastic-certificate-pem --from-file=elastic-certificate.pem && \
	rm -f elastic-certificates.p12 elastic-certificate.pem
