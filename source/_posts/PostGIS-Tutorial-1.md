---
title: PostGIS Tutorial 1 "搭建测试环境 On Mac with Homebrew"
date: 2016-11-28 15:13:54
tags: PostGIS
---
## 1.安装postgreSQL及PostGIS

brew install postgre

brew install postgis

echo "" >> ~.bash_profile
echo "export PGDATA=${PGDATA}" >>.bash_profile
source ~/.bash_profile

sed -i 's/#logging_collector = off/logging_collector = on/' ${PGDATA}/postgresql.conf

## 2.初始化数据库实例gis
pg_ctl init gis
psql gis -c "CREATE DATABASE gis;"

## 3.在数据库实例gis中注册PostGIS扩展件
#### Enable PostGIS (includes raster)
psql gis -c "CREATE EXTENSION postgis;""
#### Enable Topology
psql gis -c “CREATE EXTENSION postgis_topology;”
#### Enable PostGIS Advanced 3D and other geoprocessing algorithms sfcgal not available with all distributions
psql gis -c “CREATE EXTENSION postgis_sfcgal;”
#### fuzzy matching needed for Tiger
psql gis -c “CREATE EXTENSION fuzzystrmatch;“
#### rule based standardizer
psql gis -c “CREATE EXTENSION address_standardizer;”
#### example rule data set
psql gis -c “CREATE EXTENSION address_standardizer_data_us;“
#### Enable US Tiger Geocoder
psql gis -c “CREATE EXTENSION postgis_tiger_geocoder;”

## 4.从[OpenStreetMap](https://mapzen.com/data/metro-extracts/)网站下载特定城市(比如"BEIJING")"OSM PBF"格式样例数据集，保存为beijing.osm.pbf。

## 5.安装“导入osm数据至postgreSQL数据库”的工具osm2pgsql
brew install osm2pgsql

## 6.将“OSM PBF”导入数据库实例gis。
osm2pgsql -c -d gis beijing.osm.pbf