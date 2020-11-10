# Jarkom_Modul2_Lapres_D15
- Ammar Alifian Fahdan
- Lii'zza Aisyah Putri Sulistio

Membuat`topologi.sh`
`# switch
uml_switch -unix switch1 > /dev/null < /dev/null &
uml_switch -unix switch2 > /dev/null < /dev/null &

# router
xterm -T SURABAYA -e linux ubd0=SURABAYA,jarkom umid=SURABAYA eth0=tuntap,,,10.151.78.65 eth1=daemon,,,switch2 eth2=daemon,,,switch1 mem=96M &

# server
xterm -T MALANG -e linux ubd0=MALANG,jarkom umid=MALANG eth0=daemon,,,switch2 mem=128M &
xterm -T MOJOKERTO -e linux ubd0=MOJOKERTO,jarkom umid=MOJOKERTO eth0=daemon,,,switch2 mem=128M &
xterm -T PROBOLINGGO -e linux ubd0=PROBOLINGGO,jarkom umid=PROBOLINGGO eth0=daemon,,,switch2 mem=128M &

# client
xterm -T SIDOARJO -e linux ubd0=SIDOARJO,jarkom umid=SIDOARJO eth0=daemon,,,switch1 mem=96M &
xterm -T GRESIK -e linux ubd0=GRESIK,jarkom umid=GRESIK eth0=daemon,,,switch1 mem=96M &`

Membuat `bye.sh`
`uml_mconsole SURABAYA halt
uml_mconsole MALANG halt
uml_mconsole MOJOKERTO halt
uml_mconsole PROBOLINGGO halt
uml_mconsole SIDOARJO halt
uml_mconsole GRESIK halt`
