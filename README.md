# Smart-City-E-Municipalities-GIS-based-project-
Developed a comprehensive GIS database for mapping municipal infrastructure,  aiding in asset management, maintenance planning, and infrastructure development initiatives.  

ArcGIS Maps SDK for JavaScript (v4.x) + React
Display Interactive Property Parcels Map with Popups

// File: MapView.jsx
import React, { useEffect, useRef } from 'react';
import MapView from "@arcgis/core/views/MapView";
import Map from "@arcgis/core/Map";
import FeatureLayer from "@arcgis/core/layers/FeatureLayer";

const ArcGISMap = () => {
  const mapDiv = useRef(null);

  useEffect(() => {
    const map = new Map({ basemap: "topo-vector" });

    const view = new MapView({
      container: mapDiv.current,
      map: map,
      center: [77.5946, 12.9716], // Longitude, Latitude
      zoom: 12
    });

    const parcelLayer = new FeatureLayer({
      url: "https://yourserver.com/arcgis/rest/services/Parcel/FeatureServer/0",
      popupTemplate: {
        title: "{OwnerName}",
        content: "Property ID: {ParcelID}<br>Area: {Area} sq.m."
      }
    });

    map.add(parcelLayer);
  }, []);

  return <div style={{ height: "100vh" }} ref={mapDiv}></div>;
};

export default ArcGISMap;

SQL Server (Spatial Support)

Store & Query Property Parcels

-- Add geometry column to properties table
ALTER TABLE Properties
ADD geom GEOMETRY;

-- Insert with spatial location
INSERT INTO Properties (ParcelID, OwnerName, Area, geom)
VALUES ('P-101', 'Anil Vadhel', 452.5, geometry::STGeomFromText('POINT(77.5946 12.9716)', 4326));

-- Spatial query: Find all parcels within a boundary
SELECT ParcelID, OwnerName
FROM Properties
WHERE geom.STWithin(geometry::STGeomFromText('POLYGON((...))', 4326)) = 1;

PostgreSQL + PostGIS (ArcGIS Enterprise Geodatabase)
Store Utility Assets in Geodatabase

-- Sample: Create sewer line table with geometry
CREATE TABLE sewer_lines (
  id SERIAL PRIMARY KEY,
  name VARCHAR(100),
  install_date DATE,
  geom geometry(LINESTRING, 4326)
);

-- Insert sewer line
INSERT INTO sewer_lines (name, install_date, geom)
VALUES ('Sewer Line A', '2021-01-01',
ST_GeomFromText('LINESTRING(77.59 12.97, 77.60 12.97)', 4326));

