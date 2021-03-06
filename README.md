# README

## Name

Red Datasets

## Description

Red Datasets provides classes that provide common datasets such as iris dataset.

You can use datasets easily because you can access each dataset with multiple ways such as `#each` and Apache Arrow Record Batch.

## Install

```console
% gem install red-datasets
```

## Usage

Here is an example to access [Iris Data Set](https://archive.ics.uci.edu/ml/datasets/iris) by `#each`  or `Table#to_h` or `Table#fetch_values`.

```ruby
require "datasets"

iris = Datasets::Iris.new
iris.each do |record|
  p [
     record.sepal_length,
     record.sepal_width,
     record.petal_length,
     record.petal_width,
     record.class,
  ]
end
# => [5.1, 3.5, 1.4, 0.2, "Iris-setosa"]
# => [4.9, 3.0, 1.4, 0.2, "Iris-setosa"]
  :
# => [7.0, 3.2, 4.7, 1.4, "Iris-versicolor"]


iris_hash = iris.to_table.to_h
p iris_hash[:sepal_length]
# => [5.1, 4.9, .. , 7.0, ..
p iris_hash[:sepal_width]
# => [3.5, 3.0, .. , 3.2, ..
p iris_hash[:petal_length]
# => [1.4, 1.4, .. , 4.7, ..
p iris_hash[:petal_width]
# => [0.2, 0.2, .. , 1.4, ..
p iris_hash[:class]
# => ["Iris-setosa", "Iris-setosa", .. , "Iris-versicolor", ..


iris_table = iris.to_table
p iris_table.fetch_values(:sepal_length, :sepal_width, :petal_length, :petal_width).transpose
# => [[5.1, 3.5, 1.4, 0.2],
      [4.9, 3.0, 1.4, 0.2],
      :
      [7.0, 3.2, 4.7, 1.4],
      :

p iris_table[:class]
# => ["Iris-setosa", "Iris-setosa", .. , "Iris-versicolor", ..
```


Here is an example to access [The CIFAR-10/100 dataset](https://www.cs.toronto.edu/~kriz/cifar.html) by `#each`:

**CIFAR-10**

```ruby
require "datasets"

cifar = Datasets::CIFAR.new(n_classes: 10, type: :train)
cifar.metadata
#=> #<struct Datasets::Metadata name="CIFAR-10", url="https://www.cs.toronto.edu/~kriz/cifar.html", licenses=nil, description="CIFAR-10 is 32x32 image dataset">licenses=nil, description="CIFAR-10 is 32x32 image datasets">
cifar.each do |record|
  p record.pixels
  # => [59, 43, 50, 68, 98, 119, 139, 145, 149, 143, .....]
  p record.label
  # => 6
end
```

**CIFAR-100**

```ruby
require "datasets"

cifar = Datasets::CIFAR.new(n_classes: 100, type: :test)
cifar.metadata
#=> #<struct Datasets::Metadata name="CIFAR-100", url="https://www.cs.toronto.edu/~kriz/cifar.html", licenses=nil, description="CIFAR-100 is 32x32 image dataset">
cifar.each do |record|
  p record.pixels
  #=> [199, 196, 195, 195, 196, 197, 198, 198, 199, .....]
  p record.coarse_label
  #=> 10
  p record.fine_label
  #=> 49
end
```

## License

The MIT license. See `LICENSE.txt` for details.
