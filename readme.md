GestureDetection on Android using Xamarin
=========================================

Based on: https://github.com/Almeros/android-gesture-detectors

Why?
----

There is no proper gesture detection facility to use on Android or something that can be extended easily to implement these simple gestures.


Using these detectors
---------------------

The detectors follow exactly the same pattern as the existing Android ScaleGestureDetector. I realize that looking at C# standard practices there are better ways of doing this. I purposely did not do this to keep everything consistent with the existing ScaleGestureDetector.

This example using all three detectors: MoveGestureDetector, ScaleGestureDetector and RotationGestureDetector. Initialize the detectors in the initialization code:

```
...

			// initialize the gesture detection.
			this.SetOnTouchListener(this);
			_scaleGestureDetector = new ScaleGestureDetector(
				this.Context, this);
			_rotateGestureDetector = new RotateGestureDetector (
				this.Context, this);
			_moveGestureDetector = new MoveGestureDetector (
				this.Context, this);
...

```

The above code requires you to implement four interfaces: IOnTouchListener, ScaleGestureDetector.IOnScaleGestureListener, RotateGestureDetector.IOnRotateGestureListener and MoveGestureDetector.IOnMoveGestureListener

Implement the ScaleGestureDetector.IOnScaleGestureListener:


```
		private double _deltaScale;

		public bool OnScale (ScaleGestureDetector detector)
		{
			_deltaScale = detector.ScaleFactor;
			return true;
		}

		public bool OnScaleBegin (ScaleGestureDetector detector)
		{
			return true;
		}

		public void OnScaleEnd (ScaleGestureDetector detector)
		{

		}

```

Implement the RotateGestureDetector.IOnRotateGestureListener:


```
		private double _deltaDegrees;

		public bool OnRotate (RotateGestureDetector detector)
		{
			_deltaDegrees = detector.RotationDegreesDelta;
			return true;
		}

		public bool OnRotateBegin (RotateGestureDetector detector)
		{
			return true;
		}

		public void OnRotateEnd (RotateGestureDetector detector)
		{

		}
```

Implement the MoveGestureDetector.IOnMoveGestureListener:


```		
		private double _deltaX;
		private double _deltaY;

		public bool OnMove (MoveGestureDetector detector)
		{
			global::Android.Graphics.PointF d = detector.FocusDelta;
			_deltaX = d.X;
			_deltaY = d.Y;

			return true;
		}

		public bool OnMoveBegin (MoveGestureDetector detector)
		{
			_deltaX = 0;
			_deltaY = 0;

			return true;
		}

		public void OnMoveEnd (MoveGestureDetector detector)
		{

		}

```

And finally implement IOnTouchListener


```
		public bool OnTouch (global::Android.Views.View v, MotionEvent e)
		{
			_scaleGestureDetector.OnTouchEvent(e);
			_rotateGestureDetector.OnTouchEvent (e);
			_moveGestureDetector.OnTouchEvent (e);

			// do anything you want with _deltaDegrees, _deltaX, _deltaY and _deltaScale!

			return true;
		}
```

Code
----

### RotateGestureDetector

Helps finding out the rotation angle between the line of two fingers (with the 
normal or previous finger positions)

### MoveGestureDetector

Convenience gesture detector to keep it easy moving an object around with one 
ore more fingers without losing the clean usage of the gesture detector pattern.

### ScaleGestureDetector (default Android)

This one is NOT in this framework, but is gesture detector that resides in the 
Android API since API level 8 (2.2).

### BaseGestureDetector (abstract)

Abstract class that every new gesture detector class should extend.

### TwoFingerGestureDetector (abstract)

Abstract class that extends the BaseGestureDetector and that every new gesture 
detector class for two finger operations should extend.