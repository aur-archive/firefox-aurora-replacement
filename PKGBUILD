# Maintainer: fmoralesc
# Contributors: Dan Serban, L42y, aeosynth, karol_007

pkgname=firefox-aurora-replacement
pkgver=latest
pkgrel=6
pkgdesc="Aurora channel, as a replacement for regular firefox"
url=http://www.mozilla.org/projects/firefox/
arch=(i686 x86_64)
license=(MPL GPL LGPL)
provides=('firefox')
conflicts=('firefox' 'firefox-aurora')
depends=(desktop-file-utils libxt mime-types nss shared-mime-info)
source=(firefox.desktop
        firefox-safe.desktop)
md5sums=('5d343477fbd8d62f0d61f13c7781a17c'
         'd0e1e1d9208df02d1c0811d6a1725c76')

_url_prefix="ftp://ftp.mozilla.org/pub/mozilla.org/firefox/nightly/latest-mozilla-aurora/"
_l10n_url_prefix="ftp://ftp.mozilla.org/pub/mozilla.org/firefox/nightly/latest-mozilla-aurora-l10n/"
# if _lang is set to something else than the empty string, 
# we will try to fetch the corresponding localized version
_lang=""

package()
{
  msg "Finding newest version..."
  if [[ -z $_lang ]]; then
	  wget --spider --no-remove-listing --no-verbose "${_url_prefix}*linux-${CARCH}.tar.bz" 2>/dev/null
	  _file=`awk '/linux-'${CARCH}'.tar.bz/ {print $NF}' .listing | grep -v asc | tail -1 | tr -d '\r'`
  else
	  wget --spider --no-remove-listing --no-verbose "${_l10n_url_prefix}*linux-*.tar.bz" 2>/dev/null
	  _file=`awk '/'${_lang}'.linux-'${CARCH}'.tar.bz/ {print $NF}' .listing | grep -v asc | tail -1 | tr -d '\r'`
  fi
  echo "Found ${_file}"
  msg "Downloading..."
  if [[ -z $_lang ]]; then
	  wget ${_url_prefix}${_file}
  else
	  wget ${_l10n_url_prefix}${_file}
  fi
  msg "Extracting..."
  bsdtar -xf ${_file}
  mkdir -p "${pkgdir}"/{usr/{bin,share/{applications,pixmaps}},opt}
  mv firefox firefox-aurora
  mv firefox-aurora "${pkgdir}"/opt/
  ln -s /opt/firefox-aurora/firefox "${pkgdir}"/usr/bin/firefox
  install -m644 "${startdir}"/{firefox.desktop,firefox-safe.desktop} "${pkgdir}"/usr/share/applications/
}
