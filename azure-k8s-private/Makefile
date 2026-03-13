# Helpers for Azure Private Cloud AKS

SHELL := /usr/bin/env bash
.DEFAULT_GOAL := help

# Absolute path to the directory containing this Makefile
ROOT_DIR := $(dir $(abspath $(lastword $(MAKEFILE_LIST))))

# Mermaid CLI
MERMAID_CLI ?= npx @mermaid-js/mermaid-cli

# Diagram directories relative to this Makefile
DIAGRAM_BASE_DIR := $(ROOT_DIR)diagrams
DIAGRAM_SRC_DIR  := $(DIAGRAM_BASE_DIR)/src
DIAGRAM_OUT_DIR  := $(DIAGRAM_BASE_DIR)

# Discover all Mermaid source files
DIAGRAMS := $(wildcard $(DIAGRAM_SRC_DIR)/*.mmd)

.PHONY: help \
	regenerate-diagrams

help: ## Show available targets
	@awk 'BEGIN {FS = ":.*## "}; /^[a-zA-Z0-9_.-]+:.*## / {printf "%-30s %s\n", $$1, $$2}' $(MAKEFILE_LIST)

regenerate-diagrams: ## Regenerate all Mermaid diagrams from diagrams/src/*.mmd
	@set -euo pipefail; \
	mkdir -p "$(DIAGRAM_OUT_DIR)"; \
	if [ -z "$(DIAGRAMS)" ]; then \
		echo "No Mermaid source files found in $(DIAGRAM_SRC_DIR)"; \
		exit 0; \
	fi; \
	for file in $(DIAGRAMS); do \
		name="$$(basename "$$file" .mmd)"; \
		out="$(DIAGRAM_OUT_DIR)/$$name.svg"; \
		echo "Rendering $$file -> $$out"; \
		$(MERMAID_CLI) -i "$$file" -o "$$out"; \
	done
