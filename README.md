# SQL Server Spatial Tools

## Contents

- [Contents](#contents)
- [Introduction](#introduction)
- [Build\Signing](#buildsigning)
- [Installation](#installation)
- [Features](#features)

## Introduction

This project is a collection of tools for use with the spatial types in SQL Server. This project does not provide an end-user application, but rather a set of reusable functions which applications can make use of.

Development of this package is [hosted at GitHub](http://www.github.com/Microsoft/SQLServerSpatialTools). Source is available at this site.

This package is licensed under the Microsoft public license. See online [License.txt](http://www.github.com/Microsoft/SQLServerSpatialTools/blob/master/License.txt) or the included License.txt file for details.

## Build\Signing

This package contains a core assembly SQLSpatialTools.dll which can be
included and used directly from a .NET application, or can be registered
and used from within SQL Server.

### Signing

This dll is signed with Spatial.pfx file for security. So you need set the
password for the certificate before you can build from VS.

Password for pfx: `spatial@123`

To set password

1) open project properties-> Signing tab->Sign the assembly(section)
2) Click the Certificate dropdown

   You will be prompted with password text box where you need to enter the above password before build.

## Installation

Scripts for registering and unregistering the functionality in
SQL Server are included in the SQL Scripts directory.

Detailed Steps:

### To Register

1) Note the path to the SQLSpatialTools.dll file.
2) Edit the Register.sql file in the SQL Scripts directory:
      a) Insert the name of the database you are registering the
         functionality to where indicated at the beginning of the script.
      b) Insert the path to the SQLSpatialTools.dll file where indicated
         at the beginning of the script.
3) Execute the script on your SQL Server instance.

### To Unregister

1) Edit the Unregister.sql file in the SQL Scripts directory and insert
   the name of the database use are unregistering the functionality
   from where indicated at the beginning of the script.
2) Execute the script on your SQL Server instance.

## Features

### Scripts

Several scripts are included in the SQL Scripts directory. These include
scripts for registering and unregistering all of the following
components, as well as several examples of their use.

### Functions

These static methods, implemented in the class Functions, can both be
registered in SQL Server and used through T-SQL, as well as be used
directly from the CLR:

#### bool IsValidGeographyFromGeometry(SqlGeometry geometry)

Check if an input geometry can represent a valid geography without throwing an exception.
This function requires that the geometry be in longitude/latitude coordinates and that
those coordinates are in correct order in the geometry instance (i.e. latitude/longitude
not longitude/latitude). This function will return false (0) if the input geometry is not
in the correct latitude/longitude format, including a valid geography SRID.

#### bool IsValidGeographyFromText(string inputWKT, int srid)

Check if an input WKT can represent a valid geography. This function requires that
the WTK coordinate values are longitude/latitude values, in that order and that a valid
geography SRID value is supplied.  This function will not throw an exception even in
edge conditions (i.e. longitude/latitude coordinates are reversed to latitude/longitude).

#### SqlGeography MakeValidGeographyFromGeometry(SqlGeometry geometry)

Convert an input geometry instance to a valid geography instance.
This function requires that the WKT coordinate values are longitude/latitude values,
in that order and that a valid geography SRID value is supplied.

#### SqlGeography MakeValidGeographyFromText(string inputWKT, int srid)

Convert an input WKT to a valid geography instance.
This function requires that the WKT coordinate values are longitude/latitude values,
in that order and that a valid geography SRID value is supplied.

#### SqlGeography ConvexHullGeography(SqlGeography geography)

Computes ConvexHull of input geography and returns a polygon (unless all input points are colinear).

#### SqlGeography ConvexHullGeographyFromText(string inputWKT, int srid)

Computes ConvexHull of input WKT and returns a polygon (unless all input points are colinear).

This function does not require its input to be a valid geography. This function does require
that the WKT coordinate values are longitude/latitude values, in that order and that a valid
geography SRID value is supplied.

#### SqlGeography DensifyGeography(SqlGeography g, double maxAngle)

Returns a geography instance equivalent to its input, but with no two
consecutive points spaced more than maxAngle apart.

#### SqlGeography InterpolateBetweenGeog(SqlGeography start, SqlGeography end, double distance)

Takes start and end geography points and returns a new point that is a
given distance from the start toward the end.

#### SqlGeometry InterpolateBetweenGeom(SqlGeometry start, SqlGeometry end, double distance)

Takes start and end geometry points and returns a new point that is a
given distance from the start toward the end.

#### SqlGeography LocateAlongGeog(SqlGeography g, double distance)

Takes a geography linestring and finds the point a given distance along
it.

#### SqlGeometry LocateAlongGeom(SqlGeometry g, double distance)

Takes a geometry linestring and finds the point a given distance along
it.

#### SqlGeometry ShiftGeometry(SqlGeometry g, double xShift, double yShift)

Takes a geometry instance and shifts if by a given X and Y amount.

#### SqlGeometry VacuousGeographyToGeometry(SqlGeography toConvert, int targetSrid)

A special case of the equirectangular projection, taking each point

(lat,long) --> (y, x).

#### SqlGeography VacuousGeometryToGeography(SqlGeometry toConvert, int targetSrid)

The inverse of the VacuousGeographyToGeometry projection.

### Types

These types can be registered in SQL Server or used directly through the
CLR.

#### SqlProjection

This class provides an extensible access point to various projections and
inverse projections. See the file projection_example.sql for a sample of
its use. Currently supported projections are:

- Albers Equal Area
- Equirectangular
- Lambert Conformal Conic
- Mercator
- Oblique Mercator
- Tranverse Mercator
- Gnomonic

#### AffineTransform

This provides general affine transformations. See the example
transform_example.sql for a sample of its use.

### Aggregates

While implemented as classes, aggregates are essentially functions that
take a collection of inputs to a single result.

#### GeographyUnionAggregate

This aggregate finds the union of a set of geographies with an optional
additional buffer.

#### GeometryEnvelopeAggregate

This aggregate finds the envelope that contains a set of input
geometries.

### LRS Functions

Linear referencing (also called linear reference system or linear referencing system or LRS),
is a method of spatial referencing in engineering and construction, in which the locations of
physical features along a linear element are described in terms of measurements from a fixed point,
such as a milestone along a road.

#### LRS_ClipGeometrySegment

Returns clipped geometry segment after clip operation.

#### LRS_ConvertToLrsGeom

Converts a standard Geometry line string to an LRS geometric segment by adding measure information.

#### LRS_GetEndMeasure

Returns the End Measure of a geometric segment.

#### LRS_GetMergePosition

Checks if two input geometries are spatially connected.
       If connected returns the connected\merge position; else returns false.

#### LRS_GetStartMeasure

Returns the Start Measure of a geometric segment.

#### LRS_InterpolateBetweenGeom

Compute and get a new POINT with the specified measure between two input POINT of the geometric segment.

#### LRS_IsConnected

Checks if two input geometries are spatially connected.

#### LRS_IsValidPoint

Checks if a geometry segment is a valid POINT type or not.

#### LRS_LocatePointAlongGeom

Returns the POINT located at a specified measure from the start of a geometric segment.

#### LRS_MergeGeometrySegment

Returns the geometry object resulting from the concatenation of two geometric segments.

#### LRS_MergeAndResetGeometrySegments

Merges the input segments within tolerance and reset the measure of the resultant geometry segment.

#### LRS_OffsetGeometrySegments

Returns the geometric segment at a specified offset from a geometric segment.

#### LRS_PopulateGeometryMeasures

Populates the measures of all shape points based on the startMeasure and endMeasure of a geometric segment,
        overriding any previously assigned measures between the start point and end point.

#### LRS_ResetMeasure

Sets all measures of a geometric segment, including the start and end measures, to null values,
        overriding any previously assigned measures.

#### LRS_ReverseLinearGeometry

Returns a new geometric segment by reversing the measure values and the direction of the original geometric segment.

#### LRS_ScaleGeometrySegment

Returns a new geometric segment by scaling the original geometric segment
       (that is, redefining and shifting the measure values of all the points by the given shift measure).

#### LRS_SplitGeometrySegment

Splits a geometric segment into two geometric segments

#### LRS_TranslateMeasure

Returns a new geometric segment by translating the original geometric segment
       (that is, adding the measure values of all the points by the given translate measure).

#### LRS_ValidateLRSGeometry

Checks if the input LRS geometry is valid.

### Utility Functions

#### Util_PolygonToLine

Converts all polygon-type elements in a geometry to line-type elements.

#### Util_RemoveDuplicateVertices

Removes consecutive duplicate vertices and co linear points of the valid geometry bounds within the tolerance.

#### Util_Extract

Returns the geometry specified by the element index and ring index.
