

## Check License Status
```Shell
wmic path SoftwareLicensingProduct where "PartialProductKey is not null" get Name, LicenseStatus
```

License Status Codes:
0: Unlicensed
1: Licensed
2: Out-of-Box Grace Period
3: Out-of-Tolerance Grace Period
4: Non-Genuine Grace Period
5: Notification
6: Extended Grace