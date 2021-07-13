PKG = PanelTools

$(PKG).gfn: $(PKG).inp $(PKG).spec $(PKG)_sample.inp
	gretlcli --makepkg $(PKG).inp


install: $(PKG).gfn
	echo pkg install $(PKG).gfn --local | gretlcli -b -

