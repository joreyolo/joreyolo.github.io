# MNIST
#### error1 
<https://github.com/ageron/handson-ml/issues/301>

Scikit-Learn used to download MNIST from mldata.org, which was unfortunately pretty unstable, and was eventually shutdown. You can still use fetch_mldata() if you downloaded the dataset before mldata.org was shut down, as it will use the dataset stored in your cache. However, since Scikit-Learn 0.20, you should use fetch_openml() instead:

	from sklearn.datasets import fetch_openml
	mnist = fetch_openml('mnist_784', version=1, cache=True)
For most cases, this should work fine. However, it does not return the exact same dataset as fetch_mldata() did. Indeed, the targets are now strings instead of unsigned 8-bit integers, and also it returns the unsorted MNIST dataset, whereas fetch_mldata() returned the dataset sorted by target (the training set and the test set were sorted separately). In general, this is fine, but if you want to get the exact same results as before, you need to sort the dataset using the following function:

	def sort_by_target(mnist):
	    reorder_train = np.array(sorted([(target, i) for i, target in enumerate(mnist.target[:60000])]))[:, 1]
	    reorder_test = np.array(sorted([(target, i) for i, target in enumerate(mnist.target[60000:])]))[:, 1]
	    mnist.data[:60000] = mnist.data[reorder_train]
	    mnist.target[:60000] = mnist.target[reorder_train]
	    mnist.data[60000:] = mnist.data[reorder_test + 60000]
	    mnist.target[60000:] = mnist.target[reorder_test + 60000]

	try:
	    from sklearn.datasets import fetch_openml
	    mnist = fetch_openml('mnist_784', version=1, cache=True)
	    mnist.target = mnist.target.astype(np.int8) # fetch_openml() returns targets as strings
	    sort_by_target(mnist) # fetch_openml() returns an unsorted dataset
	except ImportError:
	    from sklearn.datasets import fetch_mldata
	    mnist = fetch_mldata('MNIST original')
