# Read Mode Scroll ListView
when read some on listview inside then is shouldn't scrolling

``` dart

class PositionRetainedScrollPhysics extends ScrollPhysics {
  final bool? shouldRetain;
  const PositionRetainedScrollPhysics({
    super.parent,
    this.shouldRetain = true,
  });

  @override
  PositionRetainedScrollPhysics applyTo(ScrollPhysics? ancestor) {
    return PositionRetainedScrollPhysics(
      parent: buildParent(ancestor),
      shouldRetain: shouldRetain,
    );
  }

  @override
  double adjustPositionForNewDimensions({
    required ScrollMetrics oldPosition,
    required ScrollMetrics newPosition,
    required bool isScrolling,
    required double velocity,
  }) {
    final position = super.adjustPositionForNewDimensions(
      oldPosition: oldPosition,
      newPosition: newPosition,
      isScrolling: isScrolling,
      velocity: velocity,
    );

    // Check if the new position exceeds the scrollable extents
    if (newPosition.maxScrollExtent < oldPosition.maxScrollExtent) {
      return position.clamp(
        oldPosition.minScrollExtent,
        oldPosition.maxScrollExtent,
      );
    }

    final diff = newPosition.maxScrollExtent - oldPosition.maxScrollExtent;

    if (!isScrolling && shouldRetain!) {
      return position + diff;
    } else {
      return position;
    }
  }
}


```
