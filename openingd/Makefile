#! /usr/bin/make

# Designed to be run one level up
opening-wrongdir:
	$(MAKE) -C ../.. opening-all

default: opening-all

opening-all: lightningd/lightning_openingd

# opening needs these:
LIGHTNINGD_OPENING_HEADERS_GEN :=			\
	openingd/openingd_wiregen.h

LIGHTNINGD_OPENING_HEADERS_NOGEN :=

LIGHTNINGD_OPENING_HEADERS := $(LIGHTNINGD_OPENING_HEADERS_GEN) $(LIGHTNINGD_OPENING_HEADERS_NOGEN)

LIGHTNINGD_OPENING_SRC := openingd/openingd.c	\
	$(LIGHTNINGD_OPENING_HEADERS:.h=.c)
LIGHTNINGD_OPENING_OBJS := $(LIGHTNINGD_OPENING_SRC:.c=.o)

# Control daemon uses this:
LIGHTNINGD_OPENING_CONTROL_HEADERS := $(LIGHTNINGD_OPENING_HEADERS)
LIGHTNINGD_OPENING_CONTROL_SRC := $(LIGHTNINGD_OPENING_HEADERS:.h=.c)
LIGHTNINGD_OPENING_CONTROL_OBJS := $(LIGHTNINGD_OPENING_CONTROL_SRC:.c=.o)

LIGHTNINGD_OPENING_GEN_SRC := $(filter openingd/%wiregen.c, $(LIGHTNINGD_OPENING_SRC) $(LIGHTNINGD_OPENING_CONTROL_SRC))

LIGHTNINGD_OPENING_SRC_NOGEN := $(filter-out openingd/%wiregen.c, $(LIGHTNINGD_OPENING_SRC))

# Add to headers which any object might need.
LIGHTNINGD_HEADERS_GEN += $(LIGHTNINGD_OPENING_HEADERS_GEN)
LIGHTNINGD_HEADERS_NOGEN += $(LIGHTNINGD_OPENING_HEADERS_NOGEN)

# Common source we use.
OPENINGD_COMMON_OBJS :=				\
	common/amount.o				\
	common/base32.o				\
	common/bigsize.o			\
	common/bip32.o				\
	common/channel_config.o			\
	common/channel_id.o			\
	common/crypto_state.o			\
	common/crypto_sync.o			\
	common/cryptomsg.o			\
	common/daemon.o				\
	common/daemon_conn.o			\
	common/derive_basepoints.o		\
	common/dev_disconnect.o			\
	common/features.o			\
	common/fee_states.o			\
	common/funding_tx.o			\
	common/status_wiregen.o			\
	common/peer_status_wiregen.o		\
	common/gossip_rcvd_filter.o		\
	common/gossip_store.o			\
	common/htlc_state.o			\
	common/htlc_wire.o			\
	common/initial_channel.o		\
	common/initial_commit_tx.o		\
	common/key_derive.o			\
	common/keyset.o				\
	common/memleak.o			\
	common/msg_queue.o			\
	common/node_id.o			\
	common/onionreply.o			\
	common/penalty_base.o			\
	common/per_peer_state.o			\
	common/peer_billboard.o			\
	common/peer_failed.o			\
	common/permute_tx.o			\
	common/pseudorand.o			\
	common/read_peer_msg.o			\
	common/setup.o				\
	common/status.o				\
	common/status_wire.o			\
	common/subdaemon.o			\
	common/type_to_string.o			\
	common/utils.o				\
	common/utxo.o				\
	common/version.o			\
	common/wire_error.o			\
	common/wireaddr.o			\
	gossipd/gossipd_peerd_wiregen.o		\
	lightningd/gossip_msg.o

$(LIGHTNINGD_OPENING_OBJS): $(LIGHTNINGD_HEADERS)

LIGHTNINGD_OPENING_OBJS := $(LIGHTNINGD_OPENING_SRC:.c=.o) $(LIGHTNINGD_OPENING_GEN_SRC:.c=.o)

# Make sure these depend on everything.
ALL_OBJS += $(LIGHTNINGD_OPENING_OBJS)
ALL_PROGRAMS += lightningd/lightning_openingd
ALL_GEN_HEADERS += $(LIGHTNINGD_OPENING_HEADERS_GEN)

lightningd/lightning_openingd: $(LIGHTNINGD_OPENING_OBJS) $(OPENINGD_COMMON_OBJS) $(WIRE_OBJS) $(BITCOIN_OBJS) $(LIGHTNINGD_HSM_CLIENT_OBJS)

check-source: $(LIGHTNINGD_OPENING_SRC_NOGEN:%=check-src-include-order/%)
check-source-bolt: $(LIGHTNINGD_OPENING_SRC:%=bolt-check/%) $(LIGHTNINGD_OPENING_HEADERS:%=bolt-check/%)

check-whitespace: $(LIGHTNINGD_OPENING_SRC_NOGEN:%=check-whitespace/%) $(LIGHTNINGD_OPENING_HEADERS_NOGEN:%=check-whitespace/%)

clean: opening-clean

opening-clean:
	$(RM) $(LIGHTNINGD_OPENING_OBJS) openingd/gen_*

-include openingd/test/Makefile
