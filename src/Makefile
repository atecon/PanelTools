PKG = PanelTools

$(PKG).gfn: $(PKG).inp $(PKG).spec $(PKG)_sample.inp
	# fold -s -w 80 $(PKG)_help.txt | tee $(PKG)_help.txt
	gretlcli --makepkg $(PKG).inp


install: $(PKG).gfn
	echo pkg install $(PKG).gfn --local | gretlcli -b -
	cat $(PKG)_help.md | tee ../README.md

clean:
	rm -f $(PKG).gfn
