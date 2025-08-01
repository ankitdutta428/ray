
.. _tune-env-vars:

Environment variables used by Ray Tune
--------------------------------------

Some of Ray Tune's behavior can be configured using environment variables.
These are the environment variables Ray Tune currently considers:

* **TUNE_DISABLE_AUTO_CALLBACK_LOGGERS**: Ray Tune automatically adds a CSV and
  JSON logger callback if they haven't been passed. Setting this variable to
  `1` disables this automatic creation. Please note that this will most likely
  affect analyzing your results after the tuning run.
* **TUNE_DISABLE_AUTO_INIT**: Disable automatically calling ``ray.init()`` if
  not attached to a Ray session.
* **TUNE_DISABLE_DATED_SUBDIR**: Ray Tune automatically adds a date string to experiment
  directories when the name is not specified explicitly or the trainable isn't passed
  as a string. Setting this environment variable to ``1`` disables adding these date strings.
* **TUNE_DISABLE_STRICT_METRIC_CHECKING**: When you report metrics to Tune via
  ``session.report()`` and passed a ``metric`` parameter to ``Tuner()``, a scheduler,
  or a search algorithm, Tune will error
  if the metric was not reported in the result. Setting this environment variable
  to ``1`` will disable this check.
* **TUNE_DISABLE_SIGINT_HANDLER**: Ray Tune catches SIGINT signals (e.g. sent by
  Ctrl+C) to gracefully shutdown and do a final checkpoint. Setting this variable
  to ``1`` will disable signal handling and stop execution right away. Defaults to
  ``0``.
* **TUNE_FORCE_TRIAL_CLEANUP_S**: By default, Ray Tune will gracefully terminate trials,
  letting them finish the current training step and any user-defined cleanup.
  Setting this variable to a non-zero, positive integer will cause trials to be forcefully
  terminated after a grace period of that many seconds. Defaults to ``600`` (seconds).
* **TUNE_FUNCTION_THREAD_TIMEOUT_S**: Time in seconds the function API waits
  for threads to finish after instructing them to complete. Defaults to ``2``.
* **TUNE_GLOBAL_CHECKPOINT_S**: Time in seconds that limits how often
  experiment state is checkpointed. If not, set this will default to ``'auto'``.
  ``'auto'`` measures the time it takes to snapshot the experiment state
  and adjusts the period so that ~5% of the driver's time is spent on snapshotting.
  You should set this to a fixed value (ex: ``TUNE_GLOBAL_CHECKPOINT_S=60``)
  to snapshot your experiment state every X seconds.
* **TUNE_MAX_LEN_IDENTIFIER**: Maximum length of trial subdirectory names (those
  with the parameter values in them)
* **TUNE_MAX_PENDING_TRIALS_PG**: Maximum number of pending trials when placement groups are used. Defaults
  to ``auto``, which will be updated to ``max(200, cluster_cpus * 1.1)`` for random/grid search and ``1``
  for any other search algorithms.
* **TUNE_PLACEMENT_GROUP_PREFIX**: Prefix for placement groups created by Ray Tune. This prefix is used
  e.g. to identify placement groups that should be cleaned up on start/stop of the tuning run. This is
  initialized to a unique name at the start of the first run.
* **TUNE_PLACEMENT_GROUP_RECON_INTERVAL**: How often to reconcile placement groups. Reconcilation is
  used to make sure that the number of requested placement groups and pending/running trials are in sync.
  In normal circumstances these shouldn't differ anyway, but reconcilation makes sure to capture cases when
  placement groups are manually destroyed. Reconcilation doesn't take much time, but it can add up when
  running a large number of short trials. Defaults to every ``5`` (seconds).
* **TUNE_PRINT_ALL_TRIAL_ERRORS**: If ``1``, will print all trial errors as they come up. Otherwise, errors
  will only be saved as text files to the trial directory and not printed. Defaults to ``1``.
* **TUNE_RESULT_BUFFER_LENGTH**: Ray Tune can buffer results from trainables before they are passed
  to the driver. Enabling this might delay scheduling decisions, as trainables are speculatively
  continued. Setting this to ``1`` disables result buffering. Cannot be used with ``checkpoint_at_end``.
  Defaults to disabled.
* **TUNE_RESULT_DELIM**: Delimiter used for nested entries in
  :class:`ExperimentAnalysis <ray.tune.ExperimentAnalysis>` dataframes. Defaults to ``.`` (but will be
  changed to ``/`` in future versions of Ray).
* **TUNE_RESULT_BUFFER_MAX_TIME_S**: Similarly, Ray Tune buffers results up to ``number_of_trial/10`` seconds,
  but never longer than this value. Defaults to 100 (seconds).
* **TUNE_RESULT_BUFFER_MIN_TIME_S**: Additionally, you can specify a minimum time to buffer results. Defaults to 0.
* **TUNE_WARN_THRESHOLD_S**: Threshold for logging if an Tune event loop operation takes too long. Defaults to 0.5 (seconds).
* **TUNE_WARN_INSUFFICENT_RESOURCE_THRESHOLD_S**: Threshold for throwing a warning if no active trials are in ``RUNNING`` state
  for this amount of seconds. If the Ray Tune job is stuck in this state (most likely due to insufficient resources),
  the warning message is printed repeatedly every this amount of seconds. Defaults to 60 (seconds).
* **TUNE_WARN_INSUFFICENT_RESOURCE_THRESHOLD_S_AUTOSCALER**: Threshold for throwing a warning when the autoscaler is enabled and
  if no active trials are in ``RUNNING`` state for this amount of seconds.
  If the Ray Tune job is stuck in this state (most likely due to insufficient resources), the warning message is printed
  repeatedly every this amount of seconds. Defaults to 60 (seconds).
* **TUNE_WARN_SLOW_EXPERIMENT_CHECKPOINT_SYNC_THRESHOLD_S**: Threshold for logging a warning if the experiment state syncing
  takes longer than this time in seconds. The experiment state files should be very lightweight, so this should not take longer than ~5 seconds.
  Defaults to 5 (seconds).
* **TUNE_STATE_REFRESH_PERIOD**: Frequency of updating the resource tracking from Ray. Defaults to 10 (seconds).
* **TUNE_RESTORE_RETRY_NUM**: The number of retries that are done before a particular trial's restore is determined
  unsuccessful. After that, the trial is not restored to its previous checkpoint but rather from scratch.
  Default is ``0``. While this retry counter is taking effect, per trial failure number will not be incremented, which
  is compared against ``max_failures``.
* **TUNE_ONLY_STORE_CHECKPOINT_SCORE_ATTRIBUTE**:  If set to ``1``, only the metric defined by ``checkpoint_score_attribute``
  will be stored with each ``Checkpoint``. As a result, ``Result.best_checkpoints`` will contain only this metric,
  omitting others that would normally be included. This can significantly reduce memory usage, especially when many
  checkpoints are stored or when metrics are large. Defaults to ``0`` (i.e., all metrics are stored).
* **RAY_AIR_FULL_TRACEBACKS**: If set to 1, will print full tracebacks for training functions,
  including internal code paths. Otherwise, abbreviated tracebacks that only show user code
  are printed. Defaults to 0 (disabled).
* **RAY_AIR_NEW_OUTPUT**: If set to 0, this disables
  the `experimental new console output <https://github.com/ray-project/ray/issues/36949>`_.



There are some environment variables that are mostly relevant for integrated libraries:

* **WANDB_API_KEY**: Weights and Biases API key. You can also use ``wandb login``
  instead.
