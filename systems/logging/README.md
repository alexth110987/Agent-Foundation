# Logging

**Status:** Proposed logical architecture; no runtime implementation.

The project has not selected a logging framework, storage destination, transport, or retention policy. The detailed proposal is in [Logging Architecture](LOGGING_ARCHITECTURE.md).

Logging is intended to support accountability, troubleshooting, security review, and evidence traceability. It must apply least privilege and data minimization. Never log passwords, tokens, API keys, private keys, recovery codes, or unnecessary sensitive payloads. Event acknowledgement does not prove durable persistence, and a log record does not prove the underlying assertion is true.

The architecture remains Proposed and requires owner review. No runtime, integration, or automated logging tests have been implemented or executed.
