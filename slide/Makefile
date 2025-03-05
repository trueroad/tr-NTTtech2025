# -*- coding: utf-8; -*-

.PHONY: all rel tiffsep clean dist-clean

# ターゲットのファイル名（拡張子なし）
TARGET = showing_mistakes_on_score

all: $(TARGET).pdf
rel: $(TARGET)-rel.pdf

# 使用するコマンド類の定義
LLMK = llmk
PDF_RM_TUC = pdf-rm-tuc --linearize --object-streams=generate \
	--newline-before-endstream
GS = gs
WGET = wget
SED = sed
RM = rm -f

# ターゲットの依存関係
$(TARGET).pdf: by-sa.pdf

# LaTeX ソースから TeX 出力 PDF へ
%.pdf: %.tex
	$(RM) $(<:.tex=.xmpdata)
	$(LLMK) $<

# 原ノ味フォントの ToUnicode CMap を削除
%-rel.pdf: %.pdf
	$(PDF_RM_TUC) $< $@

# 色確認用
tiffsep: $(TARGET)-rel.pdf
	$(GS) -dSAFER -dBATCH -dNOPAUSE -sDEVICE=tiffsep \
		-sOutputFile=tiff%04d.tiff $(TARGET)-rel.pdf

# EPS から PDF を生成（CC BY-SA のロゴ用）
%.pdf: %.eps
	$(GS) -dSAFER -dBATCH -dNOPAUSE -sDEVICE=pdfwrite \
		-dCompatibilityLevel=1.5 \
		-dColorConversionStrategy=/RGB \
		-dProcessColorModel=/DeviceRGB \
		-dEPSCrop \
		-sOutputFile=$@ \
		$<

# CC BY-SA のロゴ EPS を取得
by-sa.eps:
	$(WGET) \
"http://mirrors.creativecommons.org/presskit/buttons/88x31/eps/by-sa.eps"

clean:
	-$(LLMK) --clean $(TARGET).tex
	$(RM) *~ tiff*.tiff

dist-clean: clean
	-$(LLMK) --clobber $(TARGET).tex
	$(RM) $(TARGET)-rel.pdf by-sa.pdf by-sa.eps
