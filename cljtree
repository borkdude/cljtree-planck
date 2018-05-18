#!/usr/bin/env bash
"exec" "plk" "-Sdeps" "{:deps {org.clojure/tools.cli {mvn/version \"0.3.7\"}}}" "-sf" "$0" "$@"

(ns cljtree.core
  "Tree command, inspired by https://github.com/lambdaisland/birch"
  (:require [planck.io :as io]
            [clojure.string :as str]
            [clojure.pprint :refer [pprint]]
            [clojure.tools.cli :refer [parse-opts]]))

(def I-branch "│   ")

(def T-branch "├── ")

(def L-branch "└── ")

(def SPACER   "    ")

(defn file-name
  [path]
  (last (str/split path #"/")))

(defn file-tree
  [path]
  (let [children (js/PLANCK_LIST_FILES path)
        dir? (io/directory? path)]
    (cond->
        {:name (file-name path)
         :type (if dir? "directory" "file")}
      dir? (assoc :contents
                  (map file-tree children)))))

(defn render-tree
  [{:keys [:name :contents]} colorize?]
  (cons (if colorize?
          (str "\u001b[34m" name "\u001b[0m")
          name)
        (mapcat
         (fn [child index]
           (let [subtree (render-tree child colorize?)
                 last? (= index (dec (count contents)))
                 prefix-first (if last? L-branch T-branch)
                 prefix-rest  (if last? SPACER I-branch)]
             (cons (str prefix-first (first subtree))
                   (map #(str prefix-rest %) (next subtree)))))
         contents
         (range))))

(defn stats
  [file-tree]
  (apply merge-with +
         {:total 1
          :directories (case (:type file-tree)
                         "directory" 1
                         0)}
         (map stats (:contents file-tree))))

(def cli-options [["-E" "--edn" "Output tree as EDN"]
                  ["-c" "--color" "Colorize the output"]])

(defn -main [& args]
  (let [{:keys [options arguments]}
        (parse-opts args cli-options)
        path (or (first arguments)
                 ".")
        tree (file-tree path)
        {:keys [total directories]}
        (stats tree)]
    (if (:edn options)
      (pprint tree)
      (do
        (doseq [l (render-tree tree (:color options))]
          (println l))
        (println)
        (println
         (str directories " directories, "
              (- total directories)
              " files"))))))

(apply -main *command-line-args*)
