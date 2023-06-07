# penelopeysm/pokesprite

Fork of https://github.com/msikma/pokesprite where regular (i.e. non-shiny) Gen 9 sprites have been added into the `pokemon-gen8/regular` directory.

Gen 9 sprites are taken from: https://www.deviantart.com/nolo33lp/art/Pokemon-Scarlet-and-Violet-front-and-icon-sprites-942959329

Separation of the original image into individual sprites for each Pokemon uses this script (requires ImageMagick, i.e. the `convert` executable). The original image (downloaded from link above) should be saved as `sv_all.png`.

```python
import os
from PIL import Image

NCOLS = 12
LEFT_MARGIN = 16
COL_SEP = 96
ROW_SEP = 128
SIZE = 32
POKEMON = """
sprigatito floragato meowscarada fuecoco crocalor skeledirge quaxly quaxwell quaquaval
lechonk oinkologne-m oinkologne-f tarountula spidops nymble lokix pawmi pawmo
pawmot wooper-paldea clodsire tandemaus maushold-three maushold-four fidough
dachsbun smoliv dolliv arboliva squawkabilly-green squawkabilly-blue
squawkabilly-yellow squawkabilly-white nacli naclstack garganacl charcadet
armarouge ceruledge tadbulb bellibolt wattrel kilowattrel maschiff mabosstiff
shroodle grafaiai bramblin brambleghast toedscool toedscruel klawf capsakid
scovillain rellor rabsca flittle espathra tinkatink tinkatuff tinkaton wiglett
wugtrio bombirdier finizen palafin palafin-hero varoom revavroom cyclizar
orthworm glimmet glimmora greavard houndstone flamigo cetoddle cetitan veluza
dondozo tatsugiri-curly tatsugiri-droopy tatsugiri-stretchy annihilape
tauros-combat tauros-blaze tauros-aqua farigiraf dudunsparce
dudunsparce-three-segment kingambit great-tusk scream-tail brute-bonnet
flutter-mane slither-wing sandy-shocks iron-treads iron-bundle iron-hands
iron-jugulis iron-moth iron-thorns frigibax arctibax baxcalibur gimmighoul
gholdengo wo-chien chien-pao ting-lu chi-yu roaring-moon iron-valiant koraidon
miraidon""".split()

def get_box(i):
    row_pos = i % 12
    col_pos = i // 12
    left = LEFT_MARGIN + (row_pos * COL_SEP)
    right = left + SIZE
    top = col_pos * ROW_SEP
    bottom = top + SIZE
    return (left, top, right, bottom)

os.system('mkdir -p output')
for i, pkmn in enumerate(POKEMON):
    fname = f'output/{pkmn}.png'
    with Image.open("sv_all.png") as im:
        im2 = im.crop(box=get_box(i))
        im2.save(fname)
    os.system(f'convert {fname} -trim +repage {fname}')
    os.system(f'convert {fname} -bordercolor transparent -border 3 {fname}')
```
