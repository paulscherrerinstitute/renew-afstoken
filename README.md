# renew-afstoken

**Make AFS token renewal play nice with KCM backed Kerberos credential caches!**

## Issue

Kerberos is used to authenticate on AFS. When using a Kerberos Cache Manager (KCM) we had difficulties
due to the KCM not always selecting an appropriate default credential cache. We have for example seen that caches with expired TGTs have been used while next to where were caches with valid TGTs.

## Solution
The `renew-afstoken` script is started as background process doing regular token renewal and for authentication selecting a suitable credential cache out of all caches provided by the KCM. It can be run in place of `aklog`.

When AFS is set up in PAM, following configuration can be used with [OpenAFS](http://www.openafs.org/):

```
session     optional      pam_afs_session.so always_aklog program=/usr/local/bin/renew-afstoken
```

or with [AuriStor](https://www.auristor.com/filesystem/):
```
session     optional      pam_afs_session.so always_aklog aklogprogram=/usr/local/bin/renew-afstoken
```

## Authors
- Achim Gsell <achim.gsell@psi.ch>
- Konrad Bucheli <konrad.bucheli@psi.ch>

## Apropos
To solve similar problems with user sessions, we also created the PAM module [pam_single_kcm_cache](https://github.com/paulscherrerinstitute/pam_single_kcm_cache/).
