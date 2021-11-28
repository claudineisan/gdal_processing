####################################################

###
# Claudinei Oliveira-Santos
# Biologo - Ecólogo - Cientista Ambiental
# Sensoriamento remoto - Modelagem Ecossistêmica
# claudineisan@pastoepixel.com

###
# Steps to install gdal on windows using anaconda
based on this tutorial: https://www.youtube.com/watch?v=q5WzabB-5Z0

conda create --name pygdal
conda activate pygdal
conda install -c conda-forge gdal

# Check:
python
from osgeo import gdal
from osgeo import ogr
from osgeo import osr

# Observation:
To use gdal_calc.py on windows use python at the beginning of the script

# examples:
conda activate pygdal

gdalbuildvrt -r average -tr 0.008333333333330 0.008333333333330 -te -73.9916667 -33.7508328 -28.8416667 5.2658339 output.vrt input.tif
gdal_translate input.vrt output.tif -co COMPRESS=LZW -co TILED=YES -co BIGTIFF=YES -co TFW=YES
gdalwarp -overwrite -multi -wo NUM_THREADS=val/ALL_CPUS -wm 4096 -srcnodata 0 -dstnodata 0 -t_srs EPSG:4326 input.tif output.tif -co COMPRESS=LZW -co BIGTIFF=YES
gdalwarp -cutline input.shp -cwhere "Id=0" -crop_to_cutline input.tif output.tif -dstnodata 0 -co COMPRESS=LZW -co BIGTIFF=YES -co TILED=YES
gdaladdo input_output.tif 2 4 5 6 8 16 32
gdal_rasterize -ot UInt32 -tr 0.008333333333330 0.008333333333330 -te -73.9916667 -33.7508328 -28.8416667 5.2658339 -a input_variable input.shp output.tif 

python gdal_calc.py -A input_a.tif -B input_b.tif --outfile=output.tif --calc="(A + B)" --NoDataValue=0 --co="COMPRESS=LZW" --co="BIGTIFF=YES" --co="TILED=YES"
# Maybe you need to specify where is the gdal_calc.py (like C:\\anaconda3\\scripts\\gdal_calc.py)

####################################################
