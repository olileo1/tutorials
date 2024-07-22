# Supervised Example
Let's start with an illustrative example. As you will see, the example is not realisitic at all, but hopefully it is illustrative and good enough to help you understand the difference between Supervised and Unsupervised Machine Learning.

Imagine you are responsible for a package delivery service in an alternate reality, where neither GPS nor Street Maps exist. You offer to deliver packages to the main post office of every major city in your home country. Once you delivered the package to the correct post office, you don't care anymore what is happening to the package. You travelled around your country a lot, so you know all the post office of major cities by hard and you are able to recognize them on pictures.
Now your task is to recruite delivery agents, that will perform the deliveries. Your pool of applicants is also very limited. None of the applicants now the country well enough to know any of the cities. The only information they have are the pictures of the Post Offices of each city. You are lazy and you don't want to teach them anything so you say:

"I don't care how you manage it. All I care about is, that you deliver the packages to the correct post office. When you deliver a package, you have to send me a picture of the building you delivered the package to, and then I will tell you if you delivered the package correctly!"

Every applicant gets 10 packages to practice and develop their own delivery strategies. After that you will only hire the best one. So they come up with different strategies:

* Some applicants come up with the following strategy: they drive around the country randomly and ask random people on the streets how to get to the correct post office. Sometimes it works and sometimes it doesn't. Since GPS and Street Maps do not exist, it is hard to find a random person that can direct you to the correct place.
* Some applicants come up with another strategy: they go to the train station and buy a train ticket to the train station closest to the destination. From there they take a Taxi.
* Some applicants com up with this strategy: they take a plane to an airport closest to the destination and take a Taxi from there.

After their practice packages were delivered you check the pictures of every applicant and check how often they delivered the package to the correct post office. Again, you don't care how they did it and you simply hire the one that delivered the most packages correctly.

Very weird example indeed, but this example enables me to make some comparisons with Machine Learning. Imagine the following:
* You need to solve a particular forecasting problem using Machine Learning.
* You have some data, that you want to use to train Machine Learning Models and predict something. For the sake of simplicity let's say that in the end the ML Model should predict whether there is a "dog" or a "cat" on an image
* You really don't care how it works in detail. All you care about is, how good the predictions for "dog" and "cat" are

You have a classic example of a supervised learning problem. The nice thing about supervised learning problem is the following: You don't really have to know how exactly they work. Since it is supervised you can immediately check if the predictions are correct. Just as in the weird example above: You don't care how your delivery agents get the presents to the right destination. All you care about is, that it works. That is the nice things about Supervised Learning. Even if you don't know what those ML Models/Algorithms actually do, in most of the instances you can at least check how good they are at doing, what they should do.

Obviously, it helps if you actually know what is going on. Just as it is possible to optimize the **taking the train**-strategy from above using expert knowledge of the railway company in the country, it is also possible to tune any Machine Learning Algorithm, if you actually know what it does.

# Unsupervised Example
Let's get back into our hypothetical world without maps and GPS. In your home country your package delivery service was very successfull. You have your delivery agents and you build up a solid system:

* they deliver packages and send you a picture of the post office they delivered the package to
* you check the picture and since you know all post offices in the country you can immediately determine if they delivered the package correctly or not
* all you have to do is check pictures the whole day, everything else manages by itself

But now you want to extend your business and also deliver packages into the neighbouring country. The problem sounds quite similar, but with one big difference: You have never been to this other country and you also have no idea how the post offices in those cities look like. So your previous approach, that the delivery agents just send you pictures and you check if the delivery has been successfull or not, doesn't work anymore.

Now you have to think about another way of evaluating whether a strategy is good or bad. Now we are in the realm of Unsupervised Learning. That means you do not have an immediate feedback if the output from your system is correct or not. So what can you do?

The key here is to formulate hypothesis about which strategies function properly under what circumstances. For example imagine the strategy mentioned above:

* The delivery agent takes goes to the railway company and asks for a ticket to the city closest to the delivery destination
* From there the delivery agent takes a Taxi to the desired Post Office

In order to evaluate whether that might be a good strategy or not, you need to think about the circumstances under which this strategy would work. Then you can check if your circumstances are similar. For the strategy mentioned above the following conditions must be fulfilled:

* the railway company in the neighbouring country should be good at consulting your agents on where they need to go and should be able to take them there reliable
* the cities in the neighbouring country need taxi services, that can bring your agents from the train stations to the main post offices
* your agents need to be able to speak the same language as the taxi drivers
* etc.

Once you evaluated every possible strategy like this, you know what makes sense and what doesn't. Compared to your supervised problem in your home country this requires way more knowledge. Before you simple had to check pictures. Now you need to understand the delivery strategies of all your delivery agents and you also need to check if those strategies are applicable in your neighbouring country.

This is what makes Unsupervised Learning Problems so much more trickier than Supervised Learning Problems in the realm of Machine Learning. With Unsupervised Learning Problems it is not possible to just apply some strategy and then check if it functions properly or not. If the task is Clustering you can simply apply some algorithm to your data. For example perform a Gaussian Mixture Clustering. You will end up with some Cluster definitions. But if those Clusters make any sense or not, depends on whether the model assumptions for Gaussian Mixture Clustering are fulfilled or not.

So when it comes to Unsupervised Learning problems it is way more important to know the conditions required for the algorithm to work together with the skills to check if those conditions are fulfilled. Imagine teaching the **train and taxi** strategy to all your delivery agents and not checking the requirements. If for example the railway company is completely unreliable and your agents always end up at the wrong train stations, they are never delivering their packages correctly and you don't even know.
