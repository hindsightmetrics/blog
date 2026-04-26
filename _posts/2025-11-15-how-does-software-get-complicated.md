---
title: How does software get complicated?
date: 2026-04-25 12:00:00 -0700
categories: [Software Development, Performance]
tags: [clean-code, optimization, csharp]
---

## Introduction

This is pretty obvious, but as a software solution grows the more complicated the code becomes. This is due to having different requirements for the system, business models, multiple access points, data stores, etc.

I want to go over a very simple example of how a `Circle` class can go from very basic to a more complicated one in the name of performance

## Evolution of a Circle class

Let’s start with a basic `Circle` class. It has a public field for radius and two functions to get the perimeter and area.

```csharp
public class SimpleCircle
{
    public double Radius;

    public double GetPerimeter()
    {
        return Radius * 2 * Math.Pi();
    }

    public double GetArea()
    {
        return Math.Pi() * Math.Power(Radius, 2);
    }
}
```

But we all know that publicly accessible fields are a “no-no” because it makes the caller be more tightly coupled to the field, and makes it more difficult to change or improve. Let’s use `Properties`:

```csharp
public class Circle
{
    private double _radius;
    public double Radius { get; set; }

    public double Perimeter
    {
        get
        {
            return Radius * 2 * Math.Pi();
        }
    }

    public double Area
    {
        get
        {
            return Math.Pi() * Math.Power(Radius, 2);
        }
    }
}
```

Because we don’t have a constructor, we can’t enforce that the radius will have a value. Let’s add one to ensure it always has a value.

```csharp
public class Circle
{
    private double _radius;
    public double Radius { get; set; }

    public Circle(double radius)
    {
        _radius = radius;
    }

    public double Perimeter
    {
        get
        {
            return Radius * 2 * Math.Pi();
        }
    }

    public double Area
    {
        get
        {
            return Math.Pi() * Math.Power(Radius, 2);
        }
    }
}
```

Now, imagine that `Perimeter` and `Area` are long running processes so we want to store the result for quicker performance if the radius hasn’t changed (this is called caching); we can introduce new private fields to hold those values, and only recalculate if we need to:

```csharp
public class Circle
{
    private double _radius;
    public double Radius { get; set; }

    public Circle(double radius)
    {
        _radius = radius;
    }

    private double? _perimeter = null;

    public double Perimeter
    {
        get
        {
             if (_perimeter is null)
                _perimeter = _radius * 2 * Math.Pi();

            return _perimeter;
        }
    }

    private double? _area = null;
    public double Area
    {
        get
        {
            if (_area is null)
                _area = Math.Pi() * Math.Power(Radius, 2);

            return _area;
        }
    }
}
```csharp

What if we now need to change the radius of the circle? We can add a `setter`, and call it a day right? Well, we also have to reset the value for the perimeter and area fields so we recalculate when `Perimeter` and `Area` get called:

```csharp
public class Circle
{
    private double _radius;
    public double Radius
    {
        get
        {
            return _radius;
        }
        set
        {
            _radius = value;
            _area = null;
            _perimeter = null;
        }
    }

    public Circle(double radius)
    {
        _radius = radius;
    }

    private double? _perimeter = null;

    public double Perimeter
    {
        get
        {
            if (_perimeter is null)
                _perimeter = _radius * 2 * Math.Pi();

            return _perimeter;
        }
    }

    private double? _area = null;
    public double Area
    {
        get
        {
            if (_area is null)
                _area = Math.Pi() * Math.Power(Radius, 2);

            return _area;
        }
    }
}
```

Actually, working with `nulls` is risky, so let’s get rid of those and use `bool` for the recalculations:

```csharp
public class Circle
{
    private bool _calculatePerimeter;
    private bool _calculateArea;

    private double _radius;
    public double Radius
    {
        get
        {
            return _radius;
        }
        set
        {
            _radius = value;
            _calculateArea = false;
            _calculatePerimeter = false;
        }
    }

    public Circle(double radius)
    {
        _radius = radius;
        _calculatePerimeter = true;
        _calculateArea = true;
    }

    private double? _perimeter = null;

    public double Perimeter
    {
        get
        {
            if (_calculatePerimeter)
            {
                _perimeter = _radius * 2 * Math.Pi();
                _calculatePerimeter = false;
            }

            return _perimeter;
        }
    }

    private double? _area = null;
    public double Area
    {
        get
        {
            if (_calculateArea)
            {
                _area = Math.Pi() * Math.Power(Radius, 2);
                _calculateArea = false;
            }

            return _area;
        }
    }
}
```

Next, instead of having a `bool` to control when to do the calculations, we can have a `propertyChanged` boolean, and when we call either `GetPerimeter` or `GetArea` we check that boolean to see if we need to use the stored value or recalculate.

```csharp
public class Circle
{
    private bool _propertyChanged;

    private double _radius;
    public double Radius
    {
        get
        {
            return _radius;
        }
        set
        {
            _radius = value;
            _propertyChanged = true;
        }
    }

    public Circle(double radius)
    {
        _radius = radius;
        _propertyChanged = true;
    }

    private double? _perimeter = null;

    public double Perimeter
    {
        get
        {
            EnsureCalculations();
            return _perimeter;
        }
    }

    private double? _area = null;
    public double Area
    {
        get
        {
            EnsureCalculations();
            return _area;
        }
    }

    private void EnsureCalculations()
    {
        if (!_propertyChanged)
            return;

        _perimeter = _radius * 2 * Math.Pi();
        _area = Math.Pi() * Math.Power(Radius, 2);
        _propertyChanged = false;
    }
}
```

We could go on still by adding `Lazy` loading or an actual `PropertyChangeEvent` but by now the point is made. A simple Circle with a Radius property slowly became more complex with the need to add performance improvements and other business requirements.

I intentionally skipped other subjects like inheritance, abstractions, Dependency Injection, and more, because I wanted to focus on basic code.

Then there’s another layer of things to consider: KISS (Keep It Simple, Stupid) vs DRY (Don’t Repeat Yourself); sometimes Keeping things simple clashes with not repeating yourself and we have to make a decision of what is more important for those lines of code.

## Conclusion

With these small changes a simple class, with many small changes became more complex; now imagine you have a class that represents a Business Entity that as the business grows that entity has new requirements and objectives.

While optimizations may sound like a good idea, *don’t scale until you need to*. Premature optimizations will make things harder to read and at the end of the day, they might not have a big impact on actual performance.

The good thing is that we can use abstraction to protect from any one complex object affecting the rest of the system. Something calling or using a correctly abstracted class shouldn’t be affected by how the class accomplishes its goal.
