# gasless-indexer-go

Indexador de eventos de la plataforma **Gasless Vault**: un relayer de meta-transacciones EIP-712 para transferencias ERC-20 sin gas para el usuario final.

## Rol en la arquitectura

Escucha en tiempo real los eventos `MetaTransferExecuted` emitidos por `GaslessVault` vía WebSocket (`eth_subscribe`), hace backfill histórico tras reconexiones, decodifica los logs y los persiste en PostgreSQL. Implementa protección frente a reorgs marcando los registros como `UNCONFIRMED` y promoviéndolos a `CONFIRMED` tras N confirmaciones de bloque.

## Stack

- Go 1.22
- `go-ethereum` (`ethclient`, `abigen`)
- `pgx/v5` (PostgreSQL)
- Docker

## Entregables previstos

- `abi/` — bindings generados con `abigen`.
- `syncer/backfill.go` — backfill histórico vía `FilterLogs` usando `indexer_state`.
- `syncer/streamer.go` — suscripción WebSocket con reconexión y backoff exponencial.
- `processor/pipeline.go` — decodificación, inserción y gestión de confirmaciones.
