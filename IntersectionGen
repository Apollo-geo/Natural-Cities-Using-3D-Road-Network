import geopandas as gpd
from matplotlib import pyplot as plt
import shapely
from shapely.geometry import LineString, Point, MultiPoint

roads = gpd.read_file(r'E:/Roads_withoutZM.shp')
points = [] # Define an empty list to store intersections
trueIntersection = [] # Define an empty list to generate shapefile

for i in range(0, len(roads)):
    chosen = roads.query('index==@i') #Select a new road match all other roads
    chosenLayer = chosen.iat[0, 8] #Obtain the layer of selected road
    roads_edited = roads.copy().query('layer==@chosenLayer') #Make a copy of original roads to avoid changes on source data
    roads_edited.drop(index=i, inplace=True) #Delete the selected roads from whole roads to avoid self-intersection
    intersection = chosen.unary_union.intersection(roads_edited.unary_union) #Obtain intersection of selected road and other roads
    points.append(intersection) #Add intersections
    #if not isinstance(points[i],LineString):
    if isinstance(points[i], Point) or isinstance(points[i], MultiPoint):
        if hasattr(points[i], 'geoms'):
            for j in range(0, len(points[i].geoms)):
                print('No.',i,points[i][j])
                trueIntersection.append(points[i][j])
        else:
            print('No.',i,points[i])
            trueIntersection.append(points[i])
    del chosen, chosenLayer, roads_edited, intersection

output = gpd.GeoSeries(trueIntersection,crs='EPSG:4326')
output.to_file('E:trueIntersection.shp')
