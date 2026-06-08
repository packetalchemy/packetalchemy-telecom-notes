# LTE Attach Flow

## Overview

The LTE attach procedure allows a UE to register to the EPC network and establish default connectivity.

## Main Network Elements

- UE
- eNodeB
- MME
- HSS
- SGW
- PGW

## High Level Flow

1. UE sends Attach Request
2. MME authenticates subscriber with HSS
3. Default bearer is created
4. PGW allocates IP address
5. Attach Accept is sent to UE

## Notes

Detailed signaling traces will be added later.
