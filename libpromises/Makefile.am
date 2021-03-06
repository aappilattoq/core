if !BUILTIN_EXTENSIONS
 projlib_LTLIBRARIES = libpromises.la
 libpromises_la_LDFLAGS = -version-info 3:6:0 -no-undefined
else
 noinst_LTLIBRARIES = libpromises.la
endif

AM_CPPFLAGS = @CPPFLAGS@ -I$(srcdir)/../libutils -I $(srcdir)/../libcfnet \
	-I$(srcdir)/../libenv $(ENTERPRISE_CPPFLAGS)

AM_CFLAGS = @CFLAGS@ \
	$(ENTERPRISE_CFLAGS)

AM_YFLAGS = -d

libpromises_la_LIBADD = ../libutils/libutils.la ../libcfnet/libcfnet.la \
	../libenv/libenv.la $(ENTERPRISE_LDADD)

libpromises_la_SOURCES = \
        cf3parse.y cf3parse.h \
        cf3lex.l \
        acl_tools.h acl_tools_posix.c \
        actuator.c actuator.h \
        addr_lib.c addr_lib.h \
        assoc.c assoc.h \
        audit.c audit.h \
        attributes.c attributes.h \
        bootstrap.c bootstrap.h \
        cf3.defs.h \
        cf3.extern.h \
        cf3globals.c \
        chflags.c chflags.h \
        class.c class.h \
        classes.c \
        constants.c \
        conversion.c conversion.h \
        crypto.c crypto.h \
        dbm_api.c dbm_api.h dbm_priv.h \
        dbm_migration.c dbm_migration.h \
        dbm_migration_lastseen.c \
        dbm_migration_bundles.c \
        dbm_lmdb.c \
        dbm_quick.c \
        dbm_tokyocab.c \
        dir.h dir_priv.h \
        enterprise_extension.c enterprise_extension.h \
	enterprise_stubs.c \
        eval_context.c eval_context.h \
        evalfunction.c evalfunction.h \
        exec_tools.c exec_tools.h \
        expand.c expand.h \
        extensions.c extensions.h \
        files_copy.c files_copy.h \
        files_hashes.c files_hashes.h \
        files_interfaces.c files_interfaces.h \
        files_lib.c files_lib.h \
        files_names.c files_names.h \
        fncall.c fncall.h \
        generic_agent.c generic_agent.h \
        granules.c granules.h \
        instrumentation.c instrumentation.h \
        item_lib.c item_lib.h \
        iteration.c iteration.h \
        keyring.c keyring.h\
	known_dirs.c known_dirs.h \
        lastseen.c lastseen.h \
        loading.c loading.h \
        locks.c locks.h \
        logic_expressions.c logic_expressions.h \
        matching.c matching.h \
        math_eval.c math_eval.h \
        mod_access.c mod_access.h \
        mod_common.c mod_common.h \
        mod_databases.c mod_databases.h \
        mod_environ.c mod_environ.h \
        mod_exec.c mod_exec.h \
        mod_files.c mod_files.h \
        mod_measurement.c mod_measurement.h \
        mod_methods.c mod_methods.h \
        mod_outputs.c mod_outputs.h \
        mod_packages.c mod_packages.h \
        mod_process.c mod_process.h \
        mod_report.c mod_report.h \
        mod_services.c mod_services.h \
        mod_storage.c mod_storage.h \
        mod_knowledge.c mod_knowledge.h \
        mod_users.c mod_users.h \
        modes.c \
        mutex.c mutex.h \
        ornaments.c ornaments.h \
        policy.c policy.h \
        parser.c parser.h \
        parser_state.h \
        patches.c \
        pipes.h pipes.c \
        processes_select.c processes_select.h \
        process_lib.h process_unix_priv.h \
        promises.c promises.h \
        prototypes3.h \
        rlist.c rlist.h \
        scope.c scope.h \
        shared_lib.c shared_lib.h \
        signals.c signals.h \
        sort.c sort.h \
        storage_tools.c \
        string_expressions.c string_expressions.h \
        syntax.c syntax.h \
        syslog_client.c syslog_client.h \
        timeout.c timeout.h \
        unix.c unix.h \
        var_expressions.c var_expressions.h \
        vars.c vars.h \
        variable.c variable.h \
        verify_classes.c verify_classes.h \
        verify_reports.c \
        verify_vars.c verify_vars.h

if !NT

libpromises_la_SOURCES += \
        process_unix.c \
        pipes_unix.c

if LINUX
libpromises_la_SOURCES += \
        process_linux.c
endif

if AIX
libpromises_la_SOURCES += \
	process_aix.c
endif

if SOLARIS
libpromises_la_SOURCES += \
	process_solaris.c \
	solaris_ifaddrs.c solaris_ifaddrs.h
endif

if !LINUX
if !AIX
if !SOLARIS
libpromises_la_SOURCES += \
        process_unix_stub.c
endif
endif
endif

endif # !NT

if !NT
libpromises_la_SOURCES += \
        unix_dir.c
endif
if CYGWIN
libpromises_la_SOURCES += \
        unix_dir.c
endif

BUILT_SOURCES = cf3lex.c cf3parse.h cf3parse.c \
	$(srcdir)/enterprise_extension.c \
	$(srcdir)/enterprise_extension.h

ENTERPRISE_EXTENSION_GENERATE = $(SED) -f $(srcdir)/enterprise_extension.sed $< > $@
ENTERPRISE_EXTENSION_MAKE = @if test -z "$(V)"; then echo "  SED   " $@; else echo "$(ENTERPRISE_EXTENSION_GENERATE)"; fi; $(ENTERPRISE_EXTENSION_GENERATE)
$(srcdir)/enterprise_extension.c: extensions_template.c enterprise_extension.sed
	$(ENTERPRISE_EXTENSION_MAKE)
$(srcdir)/enterprise_extension.h: extensions_template.h enterprise_extension.sed
	$(ENTERPRISE_EXTENSION_MAKE)

CLEANFILES = *.gcno *.gcda $(BUILT_SOURCES)

#
# Some basic clean ups
#
MOSTLYCLEANFILES = *~ *.orig *.rej

# libutils dependency

# Automake need to be guided due to symlinking
libpromises_la_DEPENDENCIES = ../libutils/libutils.la ../libcfnet/libcfnet.la \
	../libenv/libenv.la $(ENTERPRISE_LDADD)

.PRECIOUS: ../libutils/libutils.la

../libutils/libutils.la:
	$(MAKE) -C ../libutils $(AM_MAKEFLAGS) libutils.la

# This should be run by hand to generate bootstrap.inc
bootstrap-inc:
	perl ./text2cstring.pl failsafe.cf > bootstrap.inc
